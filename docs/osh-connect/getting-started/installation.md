---
title: Installation 
sidebar_position: 1
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Installation

OSHConnect can be installed in Python and Java, using *pip* (or *poetry*) for Python, and *gradle* for Java.

**Installing OSHConnect**

**OSHConnect-Python**

OSHConnect is published to [PyPI](https://pypi.org/project/oshconnect/), but
so far **only as alpha pre-releases** (latest `0.5.1a22`) — there is no stable
release yet. `pip` and `uv` skip pre-releases by default, so you must opt in:

[*Link to the GitHub Repository*](https://github.com/Botts-Innovative-Research/OSHConnect-Python)

```bash
pip install "oshconnect==0.5.1a22"               # exact pin auto-allows the alpha
pip install --pre oshconnect                     # or: latest alpha (note below)

uv add "oshconnect==0.5.1a22"                    # exact pin auto-allows the alpha
uv add "oshconnect>=0.5.1a0" --prerelease=allow  # or: allow future alphas
```


**OSHConnect-Java**

[*Link to the GitHub Repository*](https://github.com/opensensorhub/OSHConnect-Java)

Please clone/download the git repository for OSHConnect-Java and include it as a submodule in your gradle project.

```gradle title="settings.gradle"
includeBuild('path/to/OSHConnect-Java')
```

**OSHConnect-C++**

[*Link to the GitHub Repository*](https://github.com/opensensorhub/OSHConnect-Cpp)

Please clone/download the git repository for OSHConnect-C++ and include it as a submodule in your project.

**OSHConnect-JavaScript**

[*Link to the GitHub Repository*](https://github.com/opensensorhub/osh-js)

```shell
npm install osh-js
```