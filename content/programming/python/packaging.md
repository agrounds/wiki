# Typer

https://github.com/tiangolo/typer

>Typer is a library for building CLI applications that users will **love using** and developers will **love creating**. Based on Python 3.6+ type hints.

Example of building a python package from Typer's documentation: https://typer.tiangolo.com/tutorial/package/

# pipx

https://github.com/pypa/pipx

>pipx is a tool to help you install and run end-user applications written in Python. It's roughly similar to macOS's `brew`, JavaScript's [npx](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b), and Linux's `apt`.

### Example
```
>> pipx install pycowsay
  installed package pycowsay 2.0.3, Python 3.7.3
  These apps are now globally available
    - pycowsay
done! ✨ 🌟 ✨


>> pipx list
venvs are in /home/user/.local/pipx/venvs
apps are exposed on your $PATH at /home/user/.local/bin
   package pycowsay 2.0.3, Python 3.7.3
    - pycowsay


# Now you can run pycowsay from anywhere
>> pycowsay mooo
  ____
< mooo >
  ====
         \
          \
            ^__^
            (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

#python