# LocoNet2 Contributor Guide

Thank you for your interest in contributing to the LocoNet2 library! This guide will help you get started with contributing to the project.

## Project Architecture

The LocoNet2 library is divided into two main layers:

*   **Physical Layer (`LocoNetPhy`):** This layer is responsible for the low-level communication with the LocoNet bus. It is implemented as an abstract class, with platform-specific implementations for different microcontrollers.
*   **Dispatcher Layer (`LocoNetDispatcher`):** This layer is responsible for the high-level logic of the LocoNet protocol. It provides a simple callback-based API for sending and receiving LocoNet messages.

## Coding Conventions

The LocoNet2 library follows the Google C++ Style Guide. Please ensure that your code adheres to these conventions before submitting a pull request.

We use `astyle` to automatically format the code. You can set up a pre-commit hook to automatically format your code before each commit by running the following command from the root of the repository:

```bash
ln -s ../../support/pre-commit .git/hooks/pre-commit
```

## Contributing

To contribute to the LocoNet2 library, please follow these steps:

1.  Fork the repository on GitHub.
2.  Create a new branch for your changes.
3.  Make your changes and commit them to your branch.
4.  Push your branch to your fork on GitHub.
5.  Open a pull request to the `development` branch of the main repository.

Before submitting your pull request, please ensure that you have run the tests and that they all pass.

## Testing

The LocoNet2 library includes a suite of unit tests. To run the tests, you will need to have PlatformIO Core installed.

To run the tests, use the following command:

```bash
pio test
```
