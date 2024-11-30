
<!-- cSpell:ignore SETUPTOOLS, pyproject -->

# Python Virtual Environment

1. Fork public repository (only main branch)
1. Clone the forked repository locally
1. Create new branch ("vanilla")
1. Create or use existing `requirements.txt`
   1. You cn select one or more files
   1. Select all of them
1. Shift-Ctrl-P "Python" -> "Python: Create Environment" -> Venv -> usr/bun/python3
1. select the `requirements.txt`
1. Press "ok" top continue
1. Press the "Show logs" button

Once installed make sure the virtual environment is active:

```shell
(.venv) hmf@gandalf:/mnt/ssd2/hmf/VSCodeProjects/whisper_streaming$ 
```


# Pip

[pip install a local git repository](https://stackoverflow.com/questions/14159482/pip-install-a-local-git-repository)

```shell
$ cd your-local-repo
$ pip install -e .
```
or

```shell
$ cd your-local-repo
python setup.py install develop
```

[Development Mode (a.k.a. “Editable Installs”)](https://setuptools.pypa.io/en/latest/userguide/development_mode.html)

<!-- cSpell:disable -->
```shell
$ cd your-python-project
$ python -m venv .venv
# Activate your environment with:
#      `source .venv/bin/activate` on Unix/macOS
# or   `.venv\Scripts\activate` on Windows

$ pip install --editable .

# Now you have access to your package
# as if it was installed in .venv
$ python -c "import your_python_project"
```
<!-- cSpell:enable -->

Legacy Behavior:

Newer versions of pip no longer run the fallback command python setup.py develop when the pyproject.toml file is present. This means that setting the environment variable SETUPTOOLS_ENABLE_FEATURES="legacy-editable" will have no effect when installing a package with pip.

[Can I have an editable entry in my requirements.txt?](https://stackoverflow.com/questions/72257466/can-i-have-an-editable-entry-in-my-requirements-txt)

See: 

* [Pip documentation: Structure](https://pip.pypa.io/en/stable/reference/requirements-file-format/#structure)
* [Pip documentation: Requirement Specifiers](https://pip.pypa.io/en/stable/reference/requirement-specifiers/#requirement-specifiers)


From a remote repo:

```shell
-e git+ssh://git@example.com/repo.git
```

From a a local repo:

```shell
-e file:///home/someone/repo
-e file://C:\Users\someone\repo
```


