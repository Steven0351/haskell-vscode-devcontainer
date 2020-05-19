## DevContainer for GHC, Stack, Cabal and HIE (Haskell IDE Engine)

### What is this?

This is a [DevContainer](https://code.visualstudio.com/docs/remote/containers) environment for Visual Studio Code, allowing automatically installing the Haskell compiler (GHC), Stack, Cabal, HIE (Haskell IDE Engine), and the necessary Visual Studio Code extensions to set up a Haskell development environment with zero additional effort.

### How to use this?

Follow the [Getting Started](https://code.visualstudio.com/docs/remote/containers#_getting-started) instructions to configure your Visual Studio Code and Docker to use with DevContainers.

Place the `.devcontainer` directory in the root of your project, and the next time you load the project, Visual Studio Code will prompt to re-open the project in a container:

![image](https://user-images.githubusercontent.com/601206/73298150-7bfac580-4215-11ea-81d3-a8fabab98e30.png)

**Note**: building the container might take a few minutes until all dependencies have finished downloading.

### How does it work?

Visual Studio Code supports [Developing inside a Container](https://code.visualstudio.com/docs/remote/containers) - using a Docker image as a development environment. It automates the process of creating the container image, as well as installing additional required extensions into the editor.

Pressing **Reopen in Container** will perform the automated steps to launch the container, and set up the environment.

For more information and setup, read the official documentation: https://code.visualstudio.com/docs/remote/containers

### What's in the box?

The `Dockerfile` contains the following:

1. An image, based on [`steven0351/ghc-stack-hie`](https://hub.docker.com/r/steven0351/ghc-stack-hie), a Debian image based on the  [Official Haskell Image](https://hub.docker.com/_/haskell) for GHC version 8.6.5, as well as the following:
   * Stack version 2.3.1
   * HIE ([haskell-ide-engine](https://github.com/haskell/haskell-ide-engine)) for GHC 8.6.5
2. A script to install some additional tools (such as git), as well as configuring a special user `vscode` to allow access from VSCode.

The `devcontainer.json` has some additional configuration for VSCode, in particular, the required extensions that have to be installed, and the name of the remote user (must match the one in the `Dockerfile`).

