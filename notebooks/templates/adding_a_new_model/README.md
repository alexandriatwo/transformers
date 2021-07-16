# Using cookiecutter to generate models

This folder contains templates to generate new models that fit the current API and pass all tests. It generates models in both PyTorch and TensorFlow, completes the `__init__.py` and auto-modeling files, and creates the documentation.

## Usage

Using the `cookiecutter` utility requires to have all the `dev` dependencies installed. Let's first clone the repository and install it in our environment:

\`\`\`shell script git clone [https://github.com/huggingface/transformers](https://github.com/huggingface/transformers) cd transformers pip install -e ".\[dev\]"

```text
Once the installation is done, you can use the CLI command `add-new-model` to generate your models:

```shell script
transformers-cli add-new-model
```

This should launch the `cookiecutter` package which should prompt you to fill in the configuration.

The `modelname` should be cased according to the plain text casing, i.e., BERT, RoBERTa, DeBERTa.

```text
modelname [<ModelNAME>]:
uppercase_modelname [<MODEL_NAME>]: 
lowercase_modelname [<model_name>]: 
camelcase_modelname [<ModelName>]:
```

Fill in the `authors` with your team members:

```text
authors [The HuggingFace Team]:
```

The checkpoint identifier is the checkpoint that will be used in the examples across the files. Put the name you wish, as it will appear on the modelhub. Do not forget to include the organisation.

```text
checkpoint_identifier [organisation/<model_name>-base-cased]:
```

The tokenizer should either be based on BERT if it behaves exactly like the BERT tokenizer, or a standalone otherwise.

```text
Select tokenizer_type:
1 - Based on BERT
2 - Standalone
Choose from 1, 2 [1]:
```

Once the command has finished, you should have a total of 7 new files spread across the repository:

```text
docs/source/model_doc/<model_name>.rst
src/transformers/models/<model_name>/configuration_<model_name>.py
src/transformers/models/<model_name>/modeling_<model_name>.py
src/transformers/models/<model_name>/modeling_tf_<model_name>.py
src/transformers/models/<model_name>/tokenization_<model_name>.py
tests/test_modeling_<model_name>.py
tests/test_modeling_tf_<model_name>.py
```

You can run the tests to ensure that they all pass:

```text
python -m pytest ./tests/test_*<model_name>*.py
```

Feel free to modify each file to mimic the behavior of your model.

⚠ You should be careful about the classes preceded by the following line:️

```python
# Copied from transformers.[...]
```

This line ensures that the copy does not diverge from the source. If it _should_ diverge, because the implementation is different, this line needs to be deleted. If you don't delete this line and run `make fix-copies`, your changes will be overwritten.

Once you have edited the files to fit your architecture, simply re-run the tests \(and edit them if a change is needed!\) afterwards to make sure everything works as expected.

Once the files are generated and you are happy with your changes, here's a checklist to ensure that your contribution will be merged quickly:

* You should run the `make fixup` utility to fix the style of the files and to ensure the code quality meets the

  library's standards.

* You should complete the documentation file \(`docs/source/model_doc/<model_name>.rst`\) so that your model may be

  usable.

