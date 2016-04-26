# Terminology

Need some help getting up to speed with Droid terminology? Check out the definitions below:

## Project

A **Project** is a set of targets, usually defined in a `droid.yml` file in the root of your project.

## Target

A **Target** is a sequenence of **Tasks**, that can be executed without any parameters.
One **Project** can have multiple targets, such as `build`, `test`, `deploy`, etc. You define the 
targets your project supports.

## Task

A **Task** is a **Command** with it's arguments and/or options specified. A **Task** is usually
part of a **Target**

## Command

A **Command** is something you can run. It optionally takes arguments, and/or options.
You can view the available [commands](commands) here.
