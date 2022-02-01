# Spring Cloud Contract Example

This example app shows you how to use Spring Cloud Contract to write better integration tests.

Please read [Better Integration Testing with Spring Cloud Contract][blog] to see how this app was created.

**Prerequisites:**

- **Java 11**: This project uses Java 11. OpenJDK 11 will work just as well. Installation instructions can be found on the [OpenJDK website](https://openjdk.java.net/install/). OpenJDK can also be installed using [Homebrew](https://brew.sh/). Alternatively, [SDKMAN](https://sdkman.io/) is another great option for installing and managing Java versions.
- **Okta CLI**: You'll be using Okta as an OAuth/OIDC provider to add JWT authentication and authorization to the application. You can go to [our developer site](https://developer.okta.com) to learn more about Okta. You need a free developer account for this tutorial. The Okta CLI is an easy way to register for a free Okta developer account, or log in to an existing one, and configure a Spring Boot app to use Okta as an auth provider. The [project GitHub page](https://github.com/okta/okta-cli) has installation instructions.
- **HTTPie**: This is a powerful command-line HTTP request utility that you'll use to test your REST API. Install it according to [the docs on their site](https://httpie.org/doc#installation).

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage, and secure users and roles in any application.

* [Getting Started](#getting-started)
* [Create an Okta OIDC Application](#create-an-okta-oidc-application)
* [Start the Apps](#start-the-apps)
* [Links](#links)
* [Help](#help)
* [License](#license)

## Getting Started

To install this example application, run the following commands:

```bash
git clone https://github.com/oktadev/okta-spring-cloud-contract-example.git spring-cloud-contract
cd spring-cloud-contract
```

This will get a copy of the project installed locally. Before the projects apps will run, however, you need to create an OIDC application in Okta so you can obtain an access token.

## Create an Okta OIDC Application

Next, you'll need a free Okta developer account. Install the [Okta CLI](https://cli.okta.com/) and run `okta register` to sign up for a new account. If you already have an account, run `okta login`. Then, run `okta apps create`. Select the default app name, or change it as you see fit. Choose **Single-Page App** and press **Enter**.

Use `https://oidcdebugger.com/debug` for the Redirect URI and accept the default the Logout Redirect URI of `https://oidcdebugger.com/`.

Take note of the `clientId` and `issuer` values. You'll need those to get an access token for JWT authentication.

## Generate an Access Token

You can generate an access token using [OpenID Connect Debugger](https://oidcdebugger.com/). First, you must configure your application on Okta to use OpenID Connect's implicit flow.

Run `okta login` and open the resulting URL in your browser. Go to the **Applications** section and select the application you created with the CLI. Edit its General Settings and add **Implicit (Hybrid)** as an allowed grant type, with access token enabled. Click **Save** and copy the client ID for the next step.

Now, navigate to the [OpenID Connect Debugger website](https://oidcdebugger.com/). Fill in your client ID, and use `https://{yourOktaDomain}/oauth2/default/v1/authorize` for the Authorize URI. The state field must be filled but can contain any characters. Select **token** for the response type. Click **Send Request** to continue.

Once you have an access token, set it as a `TOKEN` environment variable in a terminal window.

```bash
TOKEN=eyJraWQiOiJYa2pXdjMzTDRBYU1ZSzNGM...
```

## Start the Apps

Start each application in separate terminal windows using your IDE or `./mvnw spring-boot:run`.

Once the applications are running, you can use the access token you created earlier to make a request on the consumer (which will pass along the request to the secured producer).

```bash
http :8081/wearhat/1 "Authorization: Bearer $TOKEN"
```

You should see the following.

```bash
HTTP/1.1 200 
...

Enjoy your new Sombrero
```

You can also run `./mvnw verify` to make sure all tests are working in the consumer project.

## Links

This example uses the following open source libraries:

* [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot)
* [Spring Cloud Contract](https://spring.io/projects/spring-cloud-contract)
* [Spring Boot](https://spring.io/projects/spring-boot)
* [Spring Security](https://spring.io/projects/spring-security)

## Help

Please post any questions as comments on the [blog post][blog], or visit our [Okta Developer Forums](https://devforum.okta.com/).

## License

Apache 2.0, see [LICENSE](LICENSE).

[blog]: https://developer.okta.com/blog/2022/02/01/spring-cloud-contract
