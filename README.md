# webhook
What is webhook?

Webhook

webhook is a lightweight configurable tool written in Go, that allows you to easily create HTTP endpoints (hooks) on your server, which you can use to execute configured commands. You can also pass data from the HTTP request (such as headers, payload or query variables) to your commands. webhook also allows you to specify rules which have to be satisfied in order for the hook to be triggered.

For example, if you're using Github or Bitbucket, you can use webhook to set up a hook that runs a redeploy script for your project on your staging server, whenever you push changes to the master branch of your project.

If you use Mattermost or Slack, you can set up an "Outgoing webhook integration" or "Slash command" to run various commands on your server, which can then report back directly to you or your channels using the "Incoming webhook integrations", or the appropriate response body.

webhook aims to do nothing more than it should do, and that is:

    receive the request,
    parse the headers, payload and query variables,
    check if the specified rules for the hook are satisfied,
    and finally, pass the specified arguments to the specified command via command line arguments or via environment variables.

Everything else is the responsibility of the command's author.
Hookdoo

hookdoo

If you don't have time to waste configuring, hosting, debugging and maintaining your webhook instance, we offer a SaaS solution that has all of the capabilities webhook provides, plus a lot more, and all that packaged in a nice friendly web interface. If you are interested, find out more at hookdoo website. If you have any questions, you can contact us at info@hookdoo.com
Getting started
Installation
Building from source

To get started, first make sure you've properly set up your Go 1.12 or newer environment and then run

$ go build github.com/adnanh/webhook

to build the latest version of the webhook.
Using package manager
Snap store

Get it from the Snap Store
Ubuntu

If you are using Ubuntu linux (17.04 or later), you can install webhook using sudo apt-get install webhook which will install community packaged version.
Debian

If you are using Debian linux ("stretch" or later), you can install webhook using sudo apt-get install webhook which will install community packaged version (thanks @freeekanayaka) from https://packages.debian.org/sid/webhook
Download prebuilt binaries

Prebuilt binaries for different architectures are available at GitHub Releases.
Configuration

Next step is to define some hooks you want webhook to serve. Begin by creating an empty file named hooks.json. This file will contain an array of hooks the webhook will serve. Check Hook definition page to see the detailed description of what properties a hook can contain, and how to use them.

Let's define a simple hook named redeploy-webhook that will run a redeploy script located in /var/scripts/redeploy.sh. Make sure that your bash script has #!/bin/sh shebang on top.

Our hooks.json file will now look like this:

[
  {
    "id": "redeploy-webhook",
    "execute-command": "/var/scripts/redeploy.sh",
    "command-working-directory": "/var/webhook"
  }
]

You can now run webhook using

$ /path/to/webhook -hooks hooks.json -verbose

It will start up on default port 9000 and will provide you with one HTTP endpoint

http://yourserver:9000/hooks/redeploy-webhook

Check webhook parameters page to see how to override the ip, port and other settings such as hook hotreload, verbose output, etc, when starting the webhook.

By performing a simple HTTP GET or POST request to that endpoint, your specified redeploy script would be executed. Neat!

However, hook defined like that could pose a security threat to your system, because anyone who knows your endpoint, can send a request and execute your command. To prevent that, you can use the "trigger-rule" property for your hook, to specify the exact circumstances under which the hook would be triggered. For example, you can use them to add a secret that you must supply as a parameter in order to successfully trigger the hook. Please check out the Hook rules page for detailed list of available rules and their usage.
Multipart Form Data

webhook provides limited support the parsing of multipart form data. Multipart form data can contain two types of parts: values and files. All form values are automatically added to the payload scope. Use the parse-parameters-as-json settings to parse a given value as JSON. All files are ignored unless they match one of the following criteria:

    The Content-Type header is application/json.
    The part is named in the parse-parameters-as-json setting.

In either case, the given file part will be parsed as JSON and added to the payload map.
Templates

webhook can parse the hooks.json input file as a Go template when given the -template CLI parameter. See the Templates page for more details on template usage.
Using HTTPS

webhook by default serves hooks using http. If you want webhook to serve secure content using https, you can use the -secure flag while starting webhook. Files containing a certificate and matching private key for the server must be provided using the -cert /path/to/cert.pem and -key /path/to/key.pem flags. If the certificate is signed by a certificate authority, the cert file should be the concatenation of the server's certificate followed by the CA's certificate.

TLS version and cipher suite selection flags are available from the command line. To list available cipher suites, use the -list-cipher-suites flag. The -tls-min-version flag can be used with -list-cipher-suites.
CORS Headers

If you want to set CORS headers, you can use the -header name=value flag while starting webhook to set the appropriate CORS headers that will be returned with each response.
Interested in running webhook inside of a Docker container?

You can use almir/webhook docker image, or create your own (please read this discussion).
Examples

Check out Hook examples page for more complex examples of hooks.
Guides featuring webhook

    Webhook & JIRA by @perfecto25
    Trigger Ansible AWX job runs on SCM (e.g. git) commit by @jpmens
    Deploy using GitHub webhooks by @awea
    Setting up Automatic Deployment and Builds Using Webhooks by Will Browning
    Auto deploy your Node.js app on push to GitHub in 3 simple steps by Karolis Rusenas
    Automate Static Site Deployments with Salt, Git, and Webhooks by Linode
    Using Prometheus to Automatically Scale WebLogic Clusters on Kubernetes by Marina Kogan
    Github Pages and Jekyll - A New Platform for LACNIC Labs by Carlos Martínez Cagnazzo
    How to Deploy React Apps Using Webhooks and Integrating Slack on Ubuntu by Arslan Ud Din Shafiq
    Private webhooks by Thomas
    Adventures in webhooks by Drake
    GitHub pro tips by Spencer Lyon
    XiaoMi Vacuum + Amazon Button = Dash Cleaning by c0mmensal
    VIDEO: Gitlab CI/CD configuration using Docker and adnanh/webhook to deploy on VPS - Tutorial #1 by Yes! Let's Learn Software Engineering
    ...
    Want to add your own? Open an Issue or create a PR :-)

Community Contributions

See the webhook-contrib repository for a collections of tools and helpers related to webhook that have been contributed by the webhook community.
Need help?

Check out existing issues to see if someone else also had the same problem, or open a new one.
Support active development
Sponsors
DigitalOcean

DigitalOcean is a simple and robust cloud computing platform, designed for developers.
BrowserStack

BrowserStack is a cloud-based cross-browser testing tool that enables developers to test their websites across various browsers on different operating systems and mobile devices, without requiring users to install virtual machines, devices or emulators.

Support this project by becoming a sponsor. Your logo will show up here with a link to your website.

By contributing

This project exists thanks to all the people who contribute. Contribute!.
By giving money

    OpenCollective Backer
    OpenCollective Sponsor
    PayPal
    Patreon
    Faircode
    Flattr

Thank you to all our backers!

License

The MIT License (MIT)

Copyright (c) 2015 Adnan Hajdarevic adnanh@gmail.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
