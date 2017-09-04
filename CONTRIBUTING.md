# How to contribute

We definitely welcome patches and contribution to fastText.py!

Here are some guidelines and information about how to do so.

## Sending patches

### Getting started

1. Check out the code:

        git clone https://github.com/salestock/fastText.py.git
        cd fastText.py
        pip install -r requirements.txt

1. Create a fork of the fastText.py repository.
1. Add your fork as a remote:

        git remote add fork git@github.com:$YOURGITHUBUSERNAME/fastText.py.git

1. Make changes, commit them.
1. Run the test suite:

        make install-dev
        make test

1. Push your changes to your fork:

        git push fork ...

1. Open a pull request.

If you are using `virtualenv`, you can create the environment first before
installing the package:

    mkvirtualenv fasttext
    workon fasttext
    make install-dev

### Update the C++ source code

1. Update `NEW_VERSION` variable with the latest version of commit in the 
fasttext's [master branch](https://github.com/facebookresearch/fastText/commits/master).
1. Run `sh update-fasttext.sh`

After this step, the old version will be moved to 
`facebookresearch-fastText-$COMMIT` and the latest version will be copied to
`fasttext/cpp`.

Make sure that `fasttext/cpp/LAST_COMMIT` is match with the latest one.

### Compile the fasttext(1)

You can use this command to compile the `fastext(1)`:

    make fasttext/cpp/fasttext
    ./fasttext/cpp/fasttext --help

### Supervised model

When working on the supervised model, these are the main APIs:

```python
# Load pre-trained models
model = fasttext.load_model('model.bin')
# or create the new one
model = fasttext.supervised(textfile, output_file='model.bin')
# Run the prediction
model.predict(texts, k=1)
model.predict_proba(texts, k=1)
# Run the test
model.test(test_file)
```

`load_model` and `supervised` are defined in `fasttext/fasttext.pyx`. This is a
[Cython](http://cython.org/) code. We use Cython to call C++ function from 
ptyhon code and get back the results. For the `model.{predict, predict_proba, test}` 
are defined in `fasttext/model.py` inside the `SupervisedModel` class. This is 
a pure Python code.

There are a few Make commands that you can use:

* Run `make test/supervised.bin` to create the supervised model. You can use
this generated model `test/supervised.bin` with the `fasttext(1)`. 
* Run `make install-dev && make test-supervised-load-model` to install the package and test the `load_model` function.

## Filing Issues
When filing an issue, make sure to answer these five questions:

1. What version of Python are you using (`python --version`)?
2. What version of `fasttext` are you using (`pip list |  grep fasttext`)?
3. What operating system and processor architecture are you using?
4. What did you do?
5. What did you expect to see?
6. What did you see instead?

### Contributing code
Unless otherwise noted, the fastText.py source files are distributed under
the BSD-style license found in the LICENSE file.
