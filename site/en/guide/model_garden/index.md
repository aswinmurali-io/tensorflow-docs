# Model Garden overview

The TensorFlow Model Garden provides implementations of many state-of-the-art
machine learning (ML) models for vision and natural language processing (NLP),
as well as workflow tools to let you quickly configure and run those models on
standard datasets. Whether you are looking to benchmark performance for a
well-known model, verify the results of recently released research, or extend
existing models, the Model Garden can help you drive your ML research and
applications forward.

The Model Garden includes the following resources for machine learning
developers:

-   [**Official models**](#official) for vision and NLP, maintained by Google
    engineers
-   [**Research models**](#research) published as part of ML research papers
-   [**Training experiment framework**](#training_framework) for fast,
    declarative training configuration of official models
-   [**Specialized ML operations**](#ops) for vision and natural language
    processing (NLP)
-   [**Model training loop**](#orbit) management with Orbit

These resources are built to be used with the TensorFlow Core framework and
integrate with your existing TensorFlow development projects. Model
Garden resources are also provided under an [open
source](https://github.com/tensorflow/models/blob/master/LICENSE) license, so
you can freely extend and distribute the models and tools.

Practical ML models are computationally intensive to train and run, and may
require accelerators such as Graphical Processing Units (GPUs) and Tensor
Processing Units (TPUs). Most of the models in Model Garden were trained on
large datasets using TPUs. However, you can also train and run these models on
GPU and CPU processors.

## Model Garden models

The machine learning models in the Model Garden include full code so you can
test, train, or re-train them for research and experimentation. The Model Garden
includes two primary categories of models: *official models* and *research
models*.

### Official models {:#official}

The [Official Models](https://github.com/tensorflow/models/tree/master/official)
repository is a collection of state-of-the-art models, with a focus on
vision and natural language processing (NLP).
These models are implemented using current TensorFlow 2.x high-level
APIs. Model libraries in this repository are optimized for fast performance and
actively maintained by Google engineers. The official models include additional
metadata you can use to quickly configure experiments using the Model Garden
[training experiment framework](#training_framework).

### Research models {:#research}

The [Research Models](https://github.com/tensorflow/models/tree/master/research)
repository is a collection of models published as code resources for research
papers. These models are implemented using both TensorFlow 1.x and 2.x. Model
libraries in the research folder are supported by the code owners and the
research community.

## Training experiment framework {:#training_framework}

The Model Garden training experiment framework lets you quickly assemble and
run training experiments using its official models and standard datasets. The
training framework uses additional metadata included with the Model Garden's
official models to allow you to configure models quickly using a declarative
programming model. You can define a training experiment using Python commands in
the [TensorFlow Model library](../../api_docs/python/tfm/core)
or configure training using a YAML configuration file, like this
[example](https://github.com/tensorflow/models/blob/master/official/vision/configs/experiments/image_classification/imagenet_resnet50_tpu.yaml).

The training framework uses
[`tfm.core.base_trainer.ExperimentConfig`](../../api_docs/python/tfm/core/base_trainer/ExperimentConfig)
as the configuration object, which contains the following top-level
configuration objects:

-   [`runtime`](https://www.tensorflow.org/api_docs/python/tfm/core/base_task/RuntimeConfig):
    Defines the processing hardware, distribution strategy, and other
    performance optimizations
-   [`task`](https://www.tensorflow.org/api_docs/python/tfm/core/config_definitions/TaskConfig):
    Defines the model, training data, losses, and initialization
-   [`trainer`](https://www.tensorflow.org/api_docs/python/tfm/core/base_trainer/TrainerConfig):
    Defines the optimizer, training loops, evaluation loops, summaries, and
    checkpoints

For a complete example using the Model Garden training experiment framework,
see the
[Image classification with Model Garden](../../tutorials/images/classification_with_model_garden)
tutorial. For information on the training experiment framework, check out the
[TensorFlow Models API documentation](../../api_docs/python/tfm/core).
If you are looking for a solution to manage training loops for your model
training experiments, check out [Orbit](#orbit).

## Specialized ML operations {:#ops}

The Model Garden contains many vision and NLP operations specifically designed
to execute state-of-the-art models that run efficiently on GPUs and TPUs. Review
the TensorFlow Models Vision library API docs for a list of specialized [vision
operations](../../api_docs/python/tfm/vision). Review the
TensorFlow Models NLP Library API docs for a list of [NLP
operations](../../api_docs/python/tfm/nlp). These libraries
also include additional utility functions used for vision and NLP data
processing, training, and model execution.

## Training loops with Orbit {:#orbit}

The Orbit tool is a flexible, lightweight library designed to make it easier to
write custom training loops in TensorFlow 2.x, and works well with the Model
Garden [training experiment framework](#training_framework). Orbit handles
common model training tasks such as saving checkpoints, running model
evaluations, and setting up summary writing. It seamlessly integrates with
`tf.distribute` and supports running on different device types, including CPU,
GPU, and TPU hardware. The Orbit tool is also [open
source](https://github.com/tensorflow/models/blob/master/orbit/LICENSE), so you
can extend and adapt to your model training needs.

You generally train TensorFlow models by writing a
[custom training loop](https://www.tensorflow.org/guide/keras/writing_a_training_loop_from_scratch),
or using the high-level Keras
[Model.fit](../../api_docs/python/tf/keras/Model#fit)
function. For simple models, you can define and manage a custom training loop
with low-level TensorFlow methods such as `tf.GradientTape` or `tf.function`.
Alternatively, you can use the high-level Keras `Model.fit`.

However, if your model is complex and your training loop requires more flexible
control or customization, then you should use Orbit. You can define most of your
training loop by extending Orbit's `AbstractTrainerclass`. Learn more about the
Orbit tool in the [Orbit API documentation](../../api_docs/python/orbit).

Note: You can use the Keras API to do what Orbit does, but you must override
the TensorFlow `train_step` function or use callbacks like ModelCheckpoint or
TensorBoard. For more information about modifying the behavior of `train_step`,
check out the
[Customize what happens in Model.fit](https://www.tensorflow.org/guide/keras/customizing_what_happens_in_fit)
page.
