---
title: RUM Error Tracking
kind: documentation
beta: true
further_reading:
- link: "/real_user_monitoring/error_tracking/explorer"
  tag: "Documentation"
  text: "RUM Error Tracking Explorer"
- link: "https://github.com/DataDog/datadog-ci/tree/master/src/commands/sourcemaps"
  tag: "Documentation"
  text: "Official repository of the Datadog CLI"
- link: "/real_user_monitoring/guide/upload-javascript-source-maps"
  tag: "Guide"
  text: "Upload javascript source maps"
- link: "https://app.datadoghq.com/error-tracking"
  tag: "UI"
  text: "Error tracking"
---

{{< img src="real_user_monitoring/error_tracking/page.png" alt="Error Tracking Page"  >}}

## What is Error Tracking?

Datadog collects a lot of errors. It's critical to the health of your system to monitor these errors, but there can be so many individual error events that it’s hard to identify which ones matter the most and should be fixed first. 

Error Tracking makes it easier to monitor these errors by:

- __Grouping similar errors into issues__ to reduce the noise and help identify the most important ones.
- __Following issues over time__ to know when they first started, if they are still ongoing, and how often they are occurring.
- __Getting all the context needed in one place__ to facilitate troubleshooting the issue.

## Getting started

Error Tracking processes errors collected from the browser by the RUM SDK (errors with [source origin][1]).

To quickly get started with error tracking:

1. Download the latest version of the [RUM Browser SDK][2].
2. Configure the __version__, the __env__ and the __service__ when [initializing your SDK][3].

### Upload mapping files

The source code of some applications is obfuscated or minified when deployed to production for performance optimization and security concerns.
The consequence is that stack traces of errors fired from those applications are also obfuscated, making the troubleshooting process much more difficult.

#### Javascript source maps

Source maps are mapping files generated when minifying Javascript source code. The [Datadog CLI][4] can be used to upload those mapping files from your build directory: it scans the build directory and its subdirectories to automatically upload the source maps with their related minified files. Upload your source maps directly from your CI pipeline:

{{< tabs >}}
{{% tab "US" %}}

1. Add `@datadog/datadog-ci` to your `package.json` file (make sure to use the latest version).
2. [Create a new and dedicated Datadog API key][1] and export it as an environment variable named `DATADOG_API_KEY`.
3. Run the following command:
```bash
datadog-ci sourcemaps upload /path/to/dist \
	--service=my-service \
	--release-version=v35.2395005 \
	--minified-path-prefix=https://hostname.com/static/js
```


[1]: https://app.datadoghq.com/account/settings#api
{{% /tab %}}
{{% tab "EU" %}}

1. Add `@datadog/datadog-ci` to your `package.json` file (make sure to use the latest version).
2. [Create a new and dedicated Datadog API key][1] and export it as an environment variable named `DATADOG_API_KEY`.
3. Configure the CLI to upload files to the EU region by exporting two additonal environment variables: `export DATADOG_SITE="datadoghq.eu"` and `export DATADOG_API_HOST="api.datadoghq.eu"`.
4. Run the following command:
```bash
datadog-ci sourcemaps upload /path/to/dist \
	--service=my-service \
	--release-version=v35.2395005 \
	--minified-path-prefix=https://hostname.com/static/js
```


[1]: https://app.datadoghq.com/account/settings#api
{{% /tab %}}
{{< /tabs >}}

For Error Tracking to properly work with your source maps, you must configure your Javascript bundler so that:

-   Source maps directly include the related source code. Make sure the <code>sourcesContent</code> attribute is not empty before uploading them.
-   The size of each source map augmented with the size of the related minified file does not exceed __our limit of 50mb__. This sum can be reduced by configuring your bundler to split the source code into multiple smaller chunks ([see how to do this with WebpackJS][5]).

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /real_user_monitoring/data_collected/error#error-origins
[2]: https://www.npmjs.com/package/@datadog/browser-rum
[3]: /real_user_monitoring/browser/#initialization-parameters
[4]: https://github.com/DataDog/datadog-ci/
[5]: https://webpack.js.org/guides/code-splitting/
