LaunchDarkly SDK for C / C++ (client-side)
===================================

The LaunchDarkly C / C++ (client-side) SDK is designed primarily for use in desktop and embedded systems applications. It follows the client-side LaunchDarkly model for single-user contexts (much like our mobile or JavaScript SDKs). It is not intended for use in multi-user systems such as web servers and applications.

For using LaunchDarkly in C / C++ server-side applications, refer to our [C / C++ server-side SDK](https://github.com/launchdarkly/c-client-server-side).

Quick setup (POSIX)
-------------------

The C / C++ SDK requires a POSIX environment, and assumes that both `libcurl` and `libpthread` are installed.

Unlike other LaunchDarkly SDKs, the C SDK has no installation steps. To get started, clone [this repository](https://github.com/launchdarkly/c-client) or download a release archive from the [GitHub Releases](https://github.com/launchdarkly/c-client/releases) page. You can use the `Makefile` in this repository as a starting point for integrating this SDK into your application.

You can get the required dependencies on Ubuntu Linux with:

```
sudo apt-get update && sudo apt-get install build-essential libcurl4-openssl-dev
```

Quick setup (Windows)
---------------------

Building the SDK requires that the Visual Studio C compiler be installed. The SDK also requires libcurl.

Unlike other LaunchDarkly SDKs, the C SDK has no installation steps. To get started, clone [this repository](https://github.com/launchdarkly/c-client) or download a release archive from the [GitHub Releases](https://github.com/launchdarkly/c-client/releases) page.

You can obtain the libcurl dependency at [curl.haxx.se](https://curl.haxx.se/download/curl-7.59.0.zip). You will need to extract curl and then update `Makefile.win` with the path you saved it to. The Makefile will automatically build curl for you. Ensure you edit `Makefile.win` to indicate if you want an `x64`, or `x86` build.

Open a Visual Studio command prompt, navigate to the c-sdk directory, and run `nmake /f Makefile.win` to build the SDK libraries.

Visual Studio command prompt can be configured for multiple environments. To ensure that you are using your intended tool chain you can launch an environment with: `call "C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\Tools\VsDevCmd.bat" -host_arch=amd64 -arch=amd64`, where `arch` is your target, `host_arch` is the platform you are building on, and the path is your path to `VsDevCmd.bat`. You will need to modify the above command for your specific setup.

Getting started
---------------

Once integrated, you can follow these steps to initialize a client instance:

1. Include the LaunchDarkly SDK headers:

```C
#include "ldapi.h"
```

2. Create a new LDClient instance and user with your SDK key:

```C
    unsigned int maxwaitmilliseconds = 10 * 1000;
    LDConfig *config = LDConfigNew("YOUR_CLIENT_SIDE_KEY");
    LDUser *user = LDUserNew("YOUR_USER_KEY");
    LDClient *client = LDClientInit(config, user, maxwaitmilliseconds);
```

In most cases, you should create a single `LDClient` instance for the lifecycle of your program (a singleton pattern can be helpful here). When finished with the client (or prior to program exit), you should close the client:

```C
LDClientClose(client);
```

Your first feature flag
-----------------------

1. Create a new feature flag on your [dashboard](https://app.launchdarkly.com)
2. In your application code, use the feature's key to check whether the flag is on for each user:

```C
show_feature = LDBoolVariation(client, "your.flag.key", false);
if (show_feature) {
    // application code to show the feature
} else {
    // the code to run if the feature is off
}
```

You'll also want to ensure that the client is initialized before checking the flag:

```C
initialized = LDClientIsInitialized(client);
```

Learn more
-----------

The full API reference to the types and functions in the SDK is in [DOCS.md](https://github.com/launchdarkly/c-client/blob/master/DOCS.md).

Check out our [documentation](https://docs.launchdarkly.com) for in-depth instructions on configuring and using LaunchDarkly. You can also head straight to the [complete reference guide for this SDK](https://docs.launchdarkly.com/v2.0/docs/c-sdk-reference).

Testing
-------

We run integration tests for all our SDKs using a centralized test harness. This approach gives us the ability to test for consistency across SDKs, as well as test networking behavior in a long-running application. These tests cover each method in the SDK, and verify that event sending, flag evaluation, stream reconnection, and other aspects of the SDK all behave correctly.

Contributing
------------

We encourage pull-requests and other contributions from the community. We've also published an [SDK contributor's guide](https://docs.launchdarkly.com/v2.0/docs/sdk-contributors-guide) that provides a detailed explanation of how our SDKs work.

About LaunchDarkly
-----------

* LaunchDarkly is a continuous delivery platform that provides feature flags as a service and allows developers to iterate quickly and safely. We allow you to easily flag your features and manage them from the LaunchDarkly dashboard.  With LaunchDarkly, you can:
    * Roll out a new feature to a subset of your users (like a group of users who opt-in to a beta tester group), gathering feedback and bug reports from real-world use cases.
    * Gradually roll out a feature to an increasing percentage of users, and track the effect that the feature has on key metrics (for instance, how likely is a user to complete a purchase if they have feature A versus feature B?).
    * Turn off a feature that you realize is causing performance problems in production, without needing to re-deploy, or even restart the application with a changed configuration file.
    * Grant access to certain features based on user attributes, like payment plan (eg: users on the ‘gold’ plan get access to more features than users in the ‘silver’ plan). Disable parts of your application to facilitate maintenance, without taking everything offline.
* LaunchDarkly provides feature flag SDKs for a wide variety of languages and technologies. Check out [our documentation](https://docs.launchdarkly.com/docs) for a complete list.
* Explore LaunchDarkly
    * [launchdarkly.com](https://www.launchdarkly.com/ "LaunchDarkly Main Website") for more information
    * [docs.launchdarkly.com](https://docs.launchdarkly.com/  "LaunchDarkly Documentation") for our documentation and SDK reference guides
    * [apidocs.launchdarkly.com](https://apidocs.launchdarkly.com/  "LaunchDarkly API Documentation") for our API documentation
    * [blog.launchdarkly.com](https://blog.launchdarkly.com/  "LaunchDarkly Blog Documentation") for the latest product updates
    * [Feature Flagging Guide](https://github.com/launchdarkly/featureflags/  "Feature Flagging Guide") for best practices and strategies
