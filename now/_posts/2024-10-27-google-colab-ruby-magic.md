---
layout: post
type: post
title: "🔮 IPython Magic: Running Ruby in Google Colab"
date: 2024-10-27
comments: true
author: "Landon Gray"
published: true
tags: [ruby, python, google-colab, jupyter, ipython]
description: |
    Want to run Ruby in Google Colab? Learn how to use IPython magic commands to seamlessly integrate Ruby into your Google Colab
 notebooks, complete with code examples and implementation details.
---

### TLDR: [Try It Out Here](https://colab.research.google.com/drive/1_qUuVGWzRzGBU7qquv0-biY-Esv3Yj89?usp=sharing) 📒

## The Problem

Earlier this year, I discovered [that this Ruby integration](https://dev.to/kojix2/ruby-kernel-in-google-colaboratory-32ni) with [Google Colab](https://colab.google) had broken [due to an Ubuntu OS upgrade from 20.04 to 22.04](https://gist.github.com/ngoto/61459a11652826416d8f3ff9360f27c2?permalink_comment_id=4686866#gistcomment-4686866). While the [iruby gem](https://github.com/SciRuby/iruby) provided support previously, I wanted to see if there was another way to execute ruby code in Google Colab.


## The Solution: IPython Magic

With the help of Claude 3.5, I created a custom IPython magic command that lets you execute Ruby code directly in your Google Colab notebooks. Here's what it looks like in action:

```python
%%ruby
def hello(name)
  puts "Hello, #{name}!"
end

hello("Rubyist")
```

### Understanding IPython Magics

IPython (an interactive shell for Python with enhanced features) extends the Python language and has commands called [IPython Magics](https://ipython.readthedocs.io/en/stable/interactive/magics.html). A "magic" or "magic command" is a special command prefixed with `%` (line magic) or `%%` (cell magic) that provides functionality beyond regular Python code.

In Google Colab as well as Jupyter notebooks, cells are areas where multiple lines of code are executed. Line magic (one `%`) applies to a single line, while cell magic (two `%%`) affects all lines in the cell.

### How Does It Work?

While Google Colab normally only executes Python code (since it's running an IPython kernel), we can use IPython Magics to extend its functionality. We register a cell magic function in Python that takes any text following `%%ruby` and executes it using IRB (Interactive Ruby):

```python
@register_cell_magic
def ruby(line, cell):
    if 'reset' in line:
        ruby_session.reset()
        print("Ruby session reset")
        return
    try:
        stdout, stderr_content = ruby_session.execute(cell)
        if stdout:
            print(stdout, end='')
        elif not stderr_content:
            print("Code executed successfully with no output.")
        if stderr_content:
            print(stderr_content, file=stderr, end='')
    except Exception as e:
        print(f"Error executing Ruby code: {e}", file=stderr)
```

Using @register_cell_magic, we register a new magic command that processes Ruby code blocks, making Ruby available alongside Python in our notebook.

## The Complete Implementation

Here's the full script that enables Ruby execution in Google Colab. To use it simply copy and paste it into a Google Colab cell and run it!:

```python
from IPython.core.magic import register_cell_magic
import subprocess
import os
import re
from sys import stderr
import shutil
import atexit

class RubySession:
    def __init__(self):
        """Initialize the Ruby session and ensure Ruby is installed."""
        self.code_history = []
        
        # Install Ruby if not present
        try:
            subprocess.run(['which', 'ruby'], check=True, capture_output=True)
            print("Ruby is already installed")
        except subprocess.CalledProcessError:
            print("Ruby is not installed, attempting to install...")
            if shutil.which('apt-get'):
                subprocess.run(['apt-get', 'update', '-y'], check=True)
                subprocess.run(['apt-get', 'install', '-y', 'ruby-full'], check=True)
                print("Ruby installation complete")
            else:
                print("Error: Cannot install Ruby automatically. Please install Ruby manually.")
                raise EnvironmentError("Ruby not found and automatic installation failed.")
                    
    def is_definition(self, code):
        """Check if the code contains definitions to be stored."""
        return bool(re.match(r'\s*(\w+\s*=|def\s|class\s)', code))
                    
    def execute(self, code):
        """Execute Ruby code within the session."""
        try:
            full_code = """
begin
  # Previous code
  {}
  # New code
  {}
rescue => e
  puts "Error: #{{e.class}} - #{{e.message}}"
  puts e.backtrace
end
""".format('\n  '.join(self.code_history), code)
            result = subprocess.run(['ruby'], 
                                    input=full_code, 
                                    capture_output=True, 
                                    text=True)
            if result.returncode == 0 and self.is_definition(code):
                self.code_history.append(code)
            return result.stdout, result.stderr
        except subprocess.CalledProcessError as e:
            return '', f"Subprocess error: {e}"
        except Exception as e:
            return '', f"Unexpected error: {e}"
            
    def cleanup(self):
        """Clean up temporary files."""
        pass
        
    def reset(self):
        """Reset the session by clearing code history."""
        self.code_history = []

# Create global session
ruby_session = RubySession()

@register_cell_magic
def ruby(line, cell):
    if 'reset' in line:
        ruby_session.reset()
        print("Ruby session reset")
        return
    try:
        stdout, stderr_content = ruby_session.execute(cell)
        if stdout:
            print(stdout, end='')
        elif not stderr_content:
            print("Code executed successfully with no output.")
        if stderr_content:
            print(stderr_content, file=stderr, end='')
    except Exception as e:
        print(f"Error executing Ruby code: {e}", file=stderr)

# Register magic function and cleanup
get_ipython().register_magic_function(ruby, magic_kind='cell')
atexit.register(ruby_session.cleanup)

print("Ruby magic command ready! Use %%ruby to run Ruby code.")
print("Example:")
print("%%ruby")
print("puts 'Hello from Ruby!'")
print("\nTo reset the Ruby session, use:")
print("%%ruby reset")
print("Note: Be cautious when executing code that modifies the session state.")
```

## How to Use Ruby in Google Colab

### Getting started is simple:

1. Open a Google Colab notebook
2. Copy and paste the script above into a cell and run it
3. Test it with a simple Ruby command:
   ```ruby
   %%ruby
   puts 'Hello, World!'
   ```

That's all it takes to start running Ruby code in Google Colab!

## Future Development

While this solution works well for immediate needs, there's still room for investigation into [why the built-in IPython Ruby magic isn't working despite being referenced in the documentation](https://github.com/ipython/ipython/blob/b65cf89c05b4edb4a32b72e0c0e1b888020ee70f/examples/IPython%20Kernel/Cell%20Magics.ipynb#L38). One thing I noticed is that the IPython version in Google Colab isn't the latest and maybe the ruby magic command isn't implemented in earlier versions of the IPython source.

If you're interested in digging deeper into iruby or exploring these issues, feel free to reach out or contribute to the [repository I've created](https://github.com/thedayisntgray/ruby_colab).

## Questions or Issues?

If you encounter any problems or have questions, you can:
- Email me at landon.gray@hey.com
- Open an issue in the [Ruby Colab repository](https://github.com/thedayisntgray/ruby_colab)
```
