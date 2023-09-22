[![GitHub release](https://img.shields.io/github/release/Santandersecurityresearch/DrHeader.svg)](https://GitHub.com/Santandersecurityresearch/DrHeader/releases/)
[![Github all releases](https://img.shields.io/github/downloads/Santandersecurityresearch/DrHeader/total.svg)](https://GitHub.com/Santandersecurityresearch/DrHeader/releases/)
[![HitCount](https://hits.dwyl.com/Santandersecurityresearch/DrHeader.svg)](https://hits.dwyl.com/Santandersecurityresearch/DrHeader)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/Santandersecurityresearch/DrHeader.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/Santandersecurityresearch/DrHeader/alerts/)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/Santandersecurityresearch/DrHeader.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/Santandersecurityresearch/DrHeader/context:python)
[![MIT license](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)

![drHEADer](assets/img/hero.png)

# Welcome to drHEADer

There are a number of HTTP headers which enhance the security of a website when used. Often ignored, or unknown, these HTTP security headers help prevent common web application vulnerabilities when used.

DrHEADer helps with the audit of security headers received in response to a single request or a list of requests.

When combined with the OWASP [Application Security Verification Standard](https://github.com/OWASP/ASVS/blob/master/4.0/en/0x22-V14-Config.md) (ASVS) 4.0, it is a useful tool to include as part of an automated CI/CD pipeline which checks for missing HTTP headers.

## How Do I Install It?
drHEADer requires at least Python 3.8 to run. The easiest way to install drHEADer is to clone this repository and via a
terminal window, run the following command:

```sh
$ python3 setup.py install --user
```

This will install all the pre-requisites and you'll end up with a drHEADer executable.

## How Do I Use It?
There are two ways you could use drHEADer, depending on what you want to achieve. The easiest way is using the CLI.

### CLI
For details on using the CLI, see [CLI.md](CLI.md)

### In a Project
It is also possible to call drHEADer from within an existing project, and this is achieved like so:

```python
from drheader import Drheader

# create drheader instance
drheader_instance = Drheader(headers={'X-XSS-Protection': '1; mode=block'})

report = drheader_instance.analyze()
print(report)
```

#### Customize HTTP method and headers

By default, the tool uses **HEAD** method when making a request, but you can change that by supplying the `method` argument like this:

```python
# create drheader instance
drheader_instance = Drheader(url="http://test.com", method="POST")
```

Remember you can use any method supported by `requests` such as POST, PUT, GET and DELETE.

At the same time, you can customize the headers sent by the request. For that, you just have to use the `request_headers` argument:

```python
# create drheader instance
custom_headers = {"token": "1234aerhga"}
drheader_instance = Drheader(url="http://test.com", request_headers=custom_headers)
```

As we continue development on drHEADer, we will further enhance this functionality.

##### Other `requests` arguments

The _verify_ argument supported by `requests` can be included. The default value is set to `True`.

```python
# create drheader instance
drheader_instance = Drheader(url="http://test.com", verify=False)
```

#### Cross-Origin Isolation
The default rules in drHEADer support cross-origin isolation via the `Cross-Origin-Embedder-Policy` and
`Cross-Origin-Opener-Policy` headers. Due to the potential for this to break websites that have not yet properly
configured their sub-resources for cross-origin isolation, these validations are opt-in at analysis time. If you want to
enforce these cross-origin isolation validations, you must pass the `cross-origin-isolated` flag.

Using the CLI:
```shell
$ drheader scan single https://example.com --cross-origin-isolated
```

In a project:
```python
import drheader

drheader_instance = drheader.Drheader(url='https://example.com')
drheader_instance.analyze(cross_origin_isolated=True)
```

## How Do I Customise drHEADer Rules?

DrHEADer relies on a yaml file that defines the policy it will use when auditing security headers. The file is located at `./drheader/rules.yml`, and you can customise it to fit your particular needs. Please follow this [link](RULES.md) if you want to know more.

## Notes

* On ubuntu systems you may need to install libyaml-dev to avoid errors related to a missing yaml.h.

### Roadmap

We have a lot of ideas for drHEADer, and will push often as a result. Some of the things you'll see shortly are:

* Building on the Python library to make it easier to embed in your own projects.
* Releasing the API, which is separate from the core library - the API allows you to hit URLs or endpoints at scale
* Better integration into MiTM proxies.

## Who Is Behind It?

DrHEADer was developed by the Santander UK Security Engineering team, who are:

* David Albone
* [Javier Domínguez Ruiz](https://github.com/javixeneize)
* Fernando Cabrerizo
* [James Morris](https://github.com/actuallyjamez)
