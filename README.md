# Custom plugin development using Gradle

This tutorial describes how to get started with a basic Gradle project and create a simple plugin extension using Gradle.
The first part explains how to set up a basic Gradle "Hello World" project.
The second part helps you write a custom plugin extension using Kotlin.
If you're new to Gradle, read [Gradle docs](https://docs.gradle.org/current/userguide/getting_started_eng.html) for more information about Gradle and its features.

> **Note:** What you should know
> - A knowledge of Java or Kotlin is required.
> - A hands-on experience with IntelliJ IDEA or any modern IDE is good to have.

> **Note:** Prerequisites to get started
> - Download and install JDK on your machine. For information on how to install JDK, see [install JDK.](https://docs.oracle.com/en/java/javase/21/install/overview-jdk-installation.html) You can install any version starting with JDK version 11 or newer.
> - Download and install Gradle version 8 or newer. For information on how to install Gradle, see [install gradle](https://gradle.org/install/)
> - Download and install IntelliJ on your machine. If you don't prefer IntelliJ, you can install any other IDE of your choice.

## Set up a basic Gradle project
If you know how to create a basic Gradle project, you can skip forward to [Custom plugin development](#create-a-custom-plugin-extension)
Once you install all the prerequisites, follow the steps listed below to create and build a basic Gradle project:

1. Open IntelliJ IDEA and create a new project with preset values as shown below:

   ![Presets to create a basic project in IntelliJ IDEA](images/create_basic_project.png)

2. Open a terminal and navigate to the project folder. Run `gradle init`. This command will help set up a Gradle "Hello World" project with `Java` as the source code language and `Kotlin` as the build script language. You can choose the following presets if you're not sure of the choices.


   ![Initialize gradle project preset values_part1](images/execute_gradle_init_1.png)

   ![Initialize gradle project preset values_part2](images/execute_gradle_init_2.png)

   After you run this command, your project structure should look as shown below:

   ![Basic project structure with Gradle](images/display_basic_gradle_project_structure.png)

   Following are some important folders and files to know:
   - `app` folder: Contains the `build` and `source` folders for your project.
   - `gradle` folder: Contains code for gradle wrapper files and a `libs.versions.toml` file. The optional `libs.versions.toml` file is used as a catalog to maintain the versions of all dependencies for your project.
   - `build.gradle.kts` : The build script for the project.
   - `gradlew` and `gradlew.bat` : The gradle wrapper scripts to run on OSX and Windows platforms. These scripts ensure that the build is run with the same Gradle version as the project irrespective of the Gradle version installed on the platforms.

3. Run `gradle build` to build the project. The build should complete successfully. For any problems, refer to the console errors to debug and fix issues.

4. Run `gradle run` to run the project. It will display "Hello World!" greeting on your console:


   ![Output of a basic Hello World Gradle project run](images/run_basic_gradle.png)

## Create a custom plugin extension

The custom Greeting plugin writes the greeting message in a couple of files under the `build` folder, in addition to displaying it on your console. The files are named "a.txt" and "b.txt".<br/>

Gradle comes in with a powerful set of features like dependency management, project configuration, running builds and delivering software easily and quickly. However, if you want to perform other tasks like deploying artifacts, configuring project and enforcing standards, running certain tests
etc., you can encapsulate such logic in a Gradle plugin. Plugins are lightweight, easy to maintain and promote code re-usability. For more information, see [Gradle plugins.](https://docs.gradle.org/current/userguide/custom_plugins.html)

To create a custom plugin, you need to write an implementation of a plugin. Gradle will then instantiate the plugin and apply it using `<plugin-name>.apply()` method.

Follow the steps below to create the custom Greeting plugin. Since we're using Kotlin as the build script language, you need to write all the custom plugin code in the `build.gradle.kts` file.

1. Add a class `Greeting` as shown below. This class extends the _**DefaultTask**_, which will write the "Hello World!" message to files. Remember, all classes in Gradle are always final. So, you need to declare your class as `open` when you want to extend it later.  The class takes inputs 
   into 2 variables - `message` for the greeting message and `outputFiles` to hold the output files. It has a function `printMessage` annotated with `@TaskAnnotation` which defines the actual logic of the task.

   ![Greeting class code extending the default task executing custom logic for the plugin](images/add_greeting_class.png)

2. Add the `GreetingPluginExtension` class which defines 2 properties - `message` for the greeting message and `outputFiles` for the output files. If you want to set the values for the properties in this class itself and not allow anyone to override them, declare them with the `val` keyword. If you want to configure these properties later, declare them with the `var` keyword.

   ![GreetingPluginExtension code defining properties required for the task](images/add_greeting_plugin_extension_class.png)

3. Add the plugin definition code in a `GreetingPlugin` class. The plugin will create a `GreetingPluginExtension` object and set up a task `hello` which uses the configuration and executes the logic of writing the message to output files. The `hello` task will be included in the task-group
   `greeting`.

   ![GreetingPlugin class code which sets up the 'hello' task for Greeting Plugin](images/add_greeting_plugin_class.png)

4. Configure the `GreetingPluginExtension` to set values for its properties.

   ![Configuring properties for the GreetingPluginExtension object](images/configure_greeting_plugin.png)

5. Add the `buildFile` function which configures the location for the "a.txt" and "b.txt" files. Apply the plugin using `apply<GreetingPlugin>()` at the top of your build file.

   ![Apply plugin command](images/apply_plugin.png)

6. Build the project and run `gradle tasks` to check if the new task `hello' is now included in your project's tasks.

   ![Listing tasks and checking if the greeting task is included](images/list_gradle_tasks.png)

7. You can also run a build scan using `gradle build --scan` to share your build reports to other teams.

8. Run the `hello` task using `gradle -q hello`. This command will write your greeting message to the output files "a.txt" and "b.txt" under your build folder.

   ![Output of the Greeting plugin execution](images/run_custom_plugin.png)


 
