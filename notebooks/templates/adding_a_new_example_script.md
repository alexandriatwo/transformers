# How to add a new example script in 🤗 Transformers

This folder provide a template for adding a new example script implementing a training or inference task with the models in the 🤗 Transformers library. To use it, you will need to install cookiecutter:

```text
pip install cookiecutter
```

or refer to the installation page of the [cookiecutter documentation](https://cookiecutter.readthedocs.io/).

You can then run the following command inside the `examples` folder of the transformers repo:

```text
cookiecutter ../templates/adding_a_new_example_script/
```

and answer the questions asked, which will generate a new folder where you will find a pre-filled template for your example following the best practices we recommend for them.

Adjust the way the data is preprocessed, the model is loaded or the Trainer is instantiated then when you're happy, add a `README.md` in the folder \(or complete the existing one if you added a script to an existing folder\) telling a user how to run your script.

Make a PR to the 🤗 Transformers repo. Don't forget to tweet about your new example with a carbon screenshot of how to run it and tag @huggingface!

