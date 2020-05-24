## DevContainer for GHC, Stack, Cabal, and HIE (Haskell IDE Engine) || HLS (Haskell Language Server)

### What is this?

This is a [DevContainer](https://code.visualstudio.com/docs/remote/containers) environment for Visual Studio Code, allowing automatically installing the Haskell compiler (GHC), Stack, Cabal, and HIE (Haskell IDE Engine) || HLS (Haskell Language Server), and the necessary Visual Studio Code extensions to set up a Haskell development environment with zero additional effort.

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

1. An image, based on [`steven0351/ghc-stack-hie`](https://hub.docker.com/r/steven0351/ghc-stack-hie) or [`steven0351/ghc-stack-hls`](https://hub.docker.com/r/steven0351/ghc-stack-hls) (depending on the `devcontainer.json` configuration), which are based on the [Official Haskell Image](https://hub.docker.com/_/haskell).
2. Stack configured with `system-ghc: true` to prevent stack from installing another GHC.
3. A script to install some additional tools (such as git), as well as configuring a special user `vscode` to allow access from VSCode.

The `devcontainer.json` provides the following additional configurations:
  * build.args:
    * GHC_VERSION - Defaults to 8.8.3. supported versions are 8.6.5, and 8.8.3
    * LSP_PROVIDER - Defaults to hie. Supported providers are hie (Hakell IDE Engine) or hls (Haskell Language Server)
  * Volumes:
    * vscode-haskell-stack - A volume to store ~/.stack to prevent having to fetch and rebuild dependencies
    * vscode-haskell-cache - A volume to store ~/.cache for hie-bios generated files

It also has some additional configuration for VSCode. In particular, it configures the required extensions that have to be installed, and the name of the remote user (must match the one in the `Dockerfile`).

## Note on using Stack to generate new projects
Stack is preconfigured in the image to allow using system GHC. However, if you generate a new project in the DevContainer without specifying a resolver, it will attempt to choose the latest resolver compatible with your dependencies (if any have yet been defined). This means that Stack may download a newer GHC if the resolver is not compatible with the pre-installed GHC. This is undesireable since HIE and HLS have been compiled for the specific GHC version tagged. For GHC 8.6.5, the last Stack LTS available is 14.27. Currently, the 8.8.3 container can run `stack new` or `stack init` without any resolver arguments and use system GHC.
