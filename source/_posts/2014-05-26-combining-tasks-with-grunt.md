---
layout: post
status: publish
published: true
title: Combining tasks with Grunt
categories: 
  - phing
  - gruntjs
  - drupal
  - drush make
  - build tools
comments: []
---

I was recently asked to help out with a few build steps for a Drupal project using [Grunt](http://gruntjs.com/) as its build system. The project's `Gruntfile.js` has a `drush:make` task that utilizes [the grunt-drush package](https://github.com/nickpack/grunt-drush) to run Drush make. This task in included in a file under the tasks directory in the main repository.

#### tasks/drush.js

```javascript
module.exports = function(grunt) {

  /**
   * Define "drush" tasks.
   *
   * grunt drush:make
   *   Builds the Drush make file to the build/html directory.
   */
  grunt.loadNpmTasks('grunt-drush');
  grunt.config('drush', {
    make: {
      args: ['make', '<%= config.srcPaths.make %>'],
      dest: '<%= config.buildPaths.html %>'
    }
  });
};
```

You can see that the task contains a few instances of _variable interpolation_, such as `<%= config.srcPaths.make %>`. By convention, the values of these variables go in a file called `Gruntconfig.json` file and are set using the `grunt.initConfig` method. In addition, the configuration for the **default** task lives in a file called `Gruntfile.js`. I have put trimmed examples of each below.

#### Gruntfile.js

```javascript
module.exports = function(grunt) {

  // Initialize global configuration variables.
  var config = grunt.file.readJSON('Gruntconfig.json');
  grunt.initConfig({
    config: config
  });

  // Load all included tasks.
  grunt.loadTasks(__dirname + '/tasks');

  // Define the default task to fully build and configure the project.
  var tasksDefault = [
    'clean:default',
    'mkdir:init',
    'drush:make',
  ];
  grunt.registerTask('default', tasksDefault);
};
```

#### Gruntconfig.json

```javascript
{
  "srcPaths": {
    "make": "src/mti_cms.make"
  },
  "buildPaths": {
    "build": "build",
    "html": "build/html"
  }
}
```

As you can see, the project's `Gruntfile.js` also has a `clean:default` task to remove the built site and a `mkdir:init` task to make the build/html directory, and the three tasks are combined with `grunt.registerTask` to make the **default** task which will be run when you invoke `grunt` with no arguments.

## A small change

In Phase2's standard project build setup using [Phing](http://www.phing.info/), we have a task that will run [drush make](http://drush.ws/docs/make.txt) when the make file's modified time is newer than the built site. This allows a user to invoke the build tool and only spend the time doing a `drush make` if the Makefile has indeed changed.

The setup needed to do this in Phing is configured in XML: if an index.php file exists and it is newer than the Makefile, don't run `drush make`. Otherwise, delete the built site and run `drush make`. The necessary configuration to do this in a Phing build.xml is below.

#### build.xml

```xml
<target name="-drush-make-uptodate" depends="init" hidden="true">
  <if>
    <available file="${html}/index.php" />
    <then>
      <uptodate property="drush.makefile.uptodate"
        targetfile="${html}/index.php" srcfile="${drush.makefile}" />
    </then>
  </if>
</target>

<!-- Use drush make to build (or rebuild) the docroot -->
<target name="drush-make" depends="-drush-make-uptodate, init"
  hidden="true" unless="drush.makefile.uptodate">
  <if>
    <available file="${html}"/>
    <then>
      <echo level="info" message="Rebuilding ${html}."/>
      <delete dir="${html}" failonerror="true"/>
    </then>
  </if>

  <exec executable="drush" checkreturn="true" passthru="true" level="info">
    <arg value="make"/>
    <arg value="${drush.makefile}"/>
    <arg value="${html}"/>
  </exec>
</target>
```

You'll note that Phing also uses variable interpolation. The syntax, `${html}`, is similar to regular PHP string interpolation. By convention, parameters for a Phing build live in a `build.properties` file.

## A newer grunt

[The grunt-newer plugin](https://github.com/tschaub/grunt-newer) appears to be the proper way to handle this. It creates a new task prefixed with `newer:` to any other defined tasks. If your task has a `src` and `dest` parameter, it will check that `src` is newer than `dest` before running the task.

In my first quick testing, I added a spurious src parameter to the `drush:make` task and then invoked the `newer:drush:make` task.

```javascript
grunt.config('drush', {
  make: {
    args: ['make', '<%= config.srcPaths.make %>'],
    src: '<%= config.srcPaths.make %>',
    dest: '<%= config.buildPaths.html %>'
  }
});
```

That modification worked properly in concert with `grunt-newer` (and the `drush` task from `grunt-drush` task didn't complain about the extra `src` parameter,) but I still also needed to conditionally run the `clean:default` and `mkdir:init` only if the Makefile was newer than the built site.

## Synchronized grunting

The answer turned out to be to create a composite task using `grunt.registerTask` and `grunt.task.run` that combined the three tasks existing tasks and then use the `grunt-newer` version of that task. The solution looked much like the following.

```javascript
grunt.registerTask('drushmake', 'Delete and create the site folder, run Drush make.', function() {
  grunt.task.run('clean:default', 'mkdir:init', 'drush:make');
});
```

I could then invoke `newer:drushmake:default` in my `Gruntfile.js` and only delete and rebuild the site when there were changes to the Makefile.