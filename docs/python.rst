.. _section_python:

Using rasa NLU from python
==========================

Training Time
-------------
For creating your models, you can follow the same instructions as non-python users.
Or, you can train directly in python with a script like the following (using spacy):

.. testcode::

    import spacy
    from rasa_nlu.training_data import TrainingData
    from rasa_nlu.trainers.spacy_sklearn_trainer import SpacySklearnTrainer

    nlp = spacy.load("en")
    training_data = TrainingData('data/examples/rasa/demo-rasa.json', 'spacy_sklearn', nlp)
    trainer = SpacySklearnTrainer('en')
    trainer.train(training_data)
    model_directory = trainer.persist('./')     # Returns the directory the model is stored in


Prediction Time
---------------

You can call rasa NLU directly from your python script. 
You just have to instantiate either the SpacySklearnInterpreter or the MITIEInterpreter.
The ``metadata.json`` in your model dir contains the necessary info, so you can just do

.. testcode::

    from rasa_nlu.interpreters.spacy_sklearn_interpreter import SpacySklearnInterpreter
    from rasa_nlu.model import Metadata
    import spacy

    metadata = Metadata.load(model_directory)   # where model_directory points to the folder the model is persisted in
    nlp = spacy.load("en")
    interpreter = SpacySklearnInterpreter.load(metadata, nlp=nlp)

You can then run:

.. testcode::

    interpreter.parse(u"The text I want to understand")

which returns the same ``dict`` as the HTTP api would (without emulation).