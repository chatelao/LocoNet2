# LocoNet2 Contributor Guide

This guide provides information for developers who want to contribute to the LocoNet2 project.

## Coding Style

The LocoNet2 project uses the `astyle` code formatter to ensure a consistent coding style. You can format the code by running the following command:

```bash
astyle --options=.astylerc --suffix=none --recursive "*.h" "*.cpp" "*.ino"
```

## Pre-Commit Hook

The LocoNet2 project includes a pre-commit hook that automatically formats the code before each commit. To install the pre-commit hook, run the following command from the root of the repository:

```bash
ln -s ../../support/pre-commit .git/hooks/pre-commit
```

## Library Architecture

The LocoNet2 library is divided into two main layers:

*   **Physical Layer (`LocoNetPhy`)**: This layer is responsible for the physical transmission and reception of LocoNet messages. It is implemented as an abstract class, with concrete implementations for different hardware platforms.
*   **Dispatcher Layer (`LocoNetDispatcher`)**: This layer is responsible for dispatching LocoNet messages to the appropriate handlers. It receives messages from the physical layer and calls registered callback functions based on the message's opcode.
