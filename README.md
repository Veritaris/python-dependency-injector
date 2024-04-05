![](https://raw.githubusercontent.com/wiki/ets-labs/python-dependency-injector/img/logo.svg)

| 

[![Latest Version](https://img.shields.io/pypi/v/dependency_injector.svg)](https://pypi.org/project/dependency-injector/)

[![License](https://img.shields.io/pypi/l/dependency_injector.svg)](https://pypi.org/project/dependency-injector/)

[![Supported Python versions](https://img.shields.io/pypi/pyversions/dependency_injector.svg)](https://pypi.org/project/dependency-injector/)

[![Supported Python implementations](https://img.shields.io/pypi/implementation/dependency_injector.svg)](https://pypi.org/project/dependency-injector/)

[![Downloads](https://pepy.tech/badge/dependency-injector)](https://pepy.tech/project/dependency-injector)

[![Downloads](https://pepy.tech/badge/dependency-injector/month)](https://pepy.tech/project/dependency-injector)

[![Downloads](https://pepy.tech/badge/dependency-injector/week)](https://pepy.tech/project/dependency-injector)

[![Wheel](https://img.shields.io/pypi/wheel/dependency-injector.svg)](https://pypi.org/project/dependency-injector/)

[![Build Status](https://img.shields.io/github/actions/workflow/status/ets-labs/python-dependency-injector/tests-and-linters.yml?branch=master)](https://github.com/ets-labs/python-dependency-injector/actions)

[![Coverage Status](https://coveralls.io/repos/github/ets-labs/python-dependency-injector/badge.svg?branch=master)](https://coveralls.io/github/ets-labs/python-dependency-injector?branch=master)

# What is `Dependency Injector`?

`Dependency Injector` is a dependency injection framework for Python.

It helps implement the dependency injection principle.

Key features of the `Dependency Injector`:

-   **Providers**. Provides `Factory`, `Singleton`, `Callable`,
    `Coroutine`, `Object`, `List`, `Dict`, `Configuration`, `Resource`,
    `Dependency`, and `Selector` providers that help assemble your
    objects. See
    [Providers](https://python-dependency-injector.ets-labs.org/providers/index.html).
-   **Overriding**. Can override any provider by another provider on the
    fly. This helps in testing and configuring dev/stage environment to
    replace API clients with stubs etc. See [Provider
    overriding](https://python-dependency-injector.ets-labs.org/providers/overriding.html).
-   **Configuration**. Reads configuration from `yaml`, `ini`, and
    `json` files, `pydantic` settings, environment variables, and
    dictionaries. See [Configuration
    provider](https://python-dependency-injector.ets-labs.org/providers/configuration.html).
-   **Resources**. Helps with initialization and configuring of logging,
    event loop, thread or process pool, etc. Can be used for
    per-function execution scope in tandem with wiring. See [Resource
    provider](https://python-dependency-injector.ets-labs.org/providers/resource.html).
-   **Containers**. Provides declarative and dynamic containers. See
    [Containers](https://python-dependency-injector.ets-labs.org/containers/index.html).
-   **Wiring**. Injects dependencies into functions and methods. Helps
    integrate with other frameworks: Django, Flask, Aiohttp, Sanic,
    FastAPI, etc. See
    [Wiring](https://python-dependency-injector.ets-labs.org/wiring.html).
-   **Asynchronous**. Supports asynchronous injections. See
    [Asynchronous
    injections](https://python-dependency-injector.ets-labs.org/providers/async.html).
-   **Typing**. Provides typing stubs, `mypy`-friendly. See [Typing and
    mypy](https://python-dependency-injector.ets-labs.org/providers/typing_mypy.html).
-   **Performance**. Fast. Written in `Cython`.
-   **Maturity**. Mature and production-ready. Well-tested, documented,
    and supported.

``` python
from dependency_injector import containers, providers
from dependency_injector.wiring import Provide, inject


class Container(containers.DeclarativeContainer):

    config = providers.Configuration()

    api_client = providers.Singleton(
        ApiClient,
        api_key=config.api_key,
        timeout=config.timeout,
    )

    service = providers.Factory(
        Service,
        api_client=api_client,
    )


@inject
def main(service: Service = Provide[Container.service]) -> None:
    ...


if __name__ == "__main__":
    container = Container()
    container.config.api_key.from_env("API_KEY", required=True)
    container.config.timeout.from_env("TIMEOUT", as_=int, default=5)
    container.wire(modules=[__name__])

    main()  # <-- dependency is injected automatically

    with container.api_client.override(mock.Mock()):
        main()  # <-- overridden dependency is injected automatically
```

When you call the `main()` function the `Service` dependency is
assembled and injected automatically.

When you do testing, you call the `container.api_client.override()`
method to replace the real API client with a mock. When you call
`main()`, the mock is injected.

You can override any provider with another provider.

It also helps you in a re-configuring project for different
environments: replace an API client with a stub on the dev or stage.

With the `Dependency Injector`, object assembling is consolidated in a
container. Dependency injections are defined explicitly. This makes it
easier to understand and change how an application works.

![](https://raw.githubusercontent.com/wiki/ets-labs/python-dependency-injector/img/di-readme.svg)

Visit the docs to know more about the [Dependency injection and
inversion of control in
Python](https://python-dependency-injector.ets-labs.org/introduction/di_in_python.html).

## Installation

The package is available on the
[PyPi](https://pypi.org/project/dependency-injector/):

    pip install dependency-injector

## Documentation

The documentation is available
[here](https://python-dependency-injector.ets-labs.org/).

## Examples

Choose one of the following:

-   [Application example (single
    container)](https://python-dependency-injector.ets-labs.org/examples/application-single-container.html)
-   [Application example (multiple
    containers)](https://python-dependency-injector.ets-labs.org/examples/application-multiple-containers.html)
-   [Decoupled packages example (multiple
    containers)](https://python-dependency-injector.ets-labs.org/examples/decoupled-packages.html)
-   [Boto3
    example](https://python-dependency-injector.ets-labs.org/examples/boto3.html)
-   [Django
    example](https://python-dependency-injector.ets-labs.org/examples/django.html)
-   [Flask
    example](https://python-dependency-injector.ets-labs.org/examples/flask.html)
-   [Aiohttp
    example](https://python-dependency-injector.ets-labs.org/examples/aiohttp.html)
-   [Sanic
    example](https://python-dependency-injector.ets-labs.org/examples/sanic.html)
-   [FastAPI
    example](https://python-dependency-injector.ets-labs.org/examples/fastapi.html)
-   [FastAPI + Redis
    example](https://python-dependency-injector.ets-labs.org/examples/fastapi-redis.html)
-   [FastAPI + SQLAlchemy
    example](https://python-dependency-injector.ets-labs.org/examples/fastapi-sqlalchemy.html)

## Tutorials

Choose one of the following:

-   [Flask web application
    tutorial](https://python-dependency-injector.ets-labs.org/tutorials/flask.html)
-   [Aiohttp REST API
    tutorial](https://python-dependency-injector.ets-labs.org/tutorials/aiohttp.html)
-   [Asyncio monitoring daemon
    tutorial](https://python-dependency-injector.ets-labs.org/tutorials/asyncio-daemon.html)
-   [CLI application
    tutorial](https://python-dependency-injector.ets-labs.org/tutorials/cli.html)

## Concept

The framework stands on the [PEP20 (The Zen of
Python)](https://www.python.org/dev/peps/pep-0020/) principle:

``` bash
Explicit is better than implicit
```

You need to specify how to assemble and where to inject the dependencies
explicitly.

The power of the framework is in its simplicity. `Dependency Injector`
is a simple tool for the powerful concept.

## Frequently asked questions

What is dependency injection?

:   -   dependency injection is a principle that decreases coupling and
        increases cohesion

Why should I do the dependency injection?

:   -   your code becomes more flexible, testable, and clear 😎

How do I start applying the dependency injection?

:   -   you start writing the code following the dependency injection
        principle
    -   you register all of your application components and their
        dependencies in the container
    -   when you need a component, you specify where to inject it or get
        it from the container

What price do I pay and what do I get?

:   -   you need to explicitly specify the dependencies
    -   it will be extra work in the beginning
    -   it will payoff as project grows

Have a question?

:   -   Open a [Github
        Issue](https://github.com/ets-labs/python-dependency-injector/issues)

Found a bug?

:   -   Open a [Github
        Issue](https://github.com/ets-labs/python-dependency-injector/issues)

Want to help?

:   -   ⭐ ️ Star the `Dependency Injector` on the
        [Github](https://github.com/ets-labs/python-dependency-injector/)
    -   🆕 Start a new project with the `Dependency Injector`
    -   💬 Tell your friend about the `Dependency Injector`

Want to contribute?

:   -   🔀 Fork the project
    -   ⬅ ️ Open a pull request to the `develop` branch
