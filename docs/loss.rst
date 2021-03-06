
Gluon Loss API
**************


Overview
========

This document lists the loss API in Gluon:

This package includes several commonly used loss functions in neural
networks.

+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``L2Loss``                        | Calculates the mean squared error between output and label.                                |
+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``L1Loss``                        | Calculates the mean absolute error between output and label.                               |
+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``SoftmaxCrossEntropyLoss``       | Computes the softmax cross entropy loss.                                                   |
+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``SigmoidBinaryCrossEntropyLoss`` | The cross-entropy loss for binary classification.                                          |
+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``KLDivLoss``                     | The Kullback-Leibler divergence loss.                                                      |
+-----------------------------------+--------------------------------------------------------------------------------------------+
| ``CTCLoss``                       | Connectionist Temporal Classification Loss.                                                |
+-----------------------------------+--------------------------------------------------------------------------------------------+


API Reference
=============

losses for training neural networks

**class gluon.loss.Loss(weight, batch_axis, **kwargs)**

   Base class for loss.

   :Parameters:
      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   **hybrid_forward(F, x, *args, **kwargs)**

      Overrides to construct symbolic graph for this *Block*.

      :Parameters:
         * **x** `Symbol
           <https://mxnet.incubator.apache.org/versions/master/api/python/symbol/symbol.html#mxnet.symbol.Symbol>`_ or `NDArray
           <https://mxnet.incubator.apache.org/versions/master/api/python/ndarray/ndarray.html#mxnet.ndarray.NDArray>`_ -- The
           first input tensor.

         * ***args** (*list of Symbol* or *list of NDArray*) --
           Additional input tensors.

**class gluon.loss.L2Loss(weight=1.0, batch_axis=0, **kwargs)**

   Calculates the mean squared error between output and label.

      L = \frac{1}{2}\sum_i \Vert {output}_i - {label}_i \Vert^2.

   Output and label can have arbitrary shape as long as they have the
   same number of elements.

   :Parameters:
      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   Input shape:
      *output* is the prediction tensor. *label* is the truth tensor
      and should have the same shape as *output*. *sample_weight* is a
      matrix for per-sample weighting. Must be broadcastable to the
      same shape as loss. For example, if loss has shape (64, 10) and
      you want to weigh each sample in the batch separately,
      *sample_weight* should have shape (64, 1).

   Output shape:
      The loss output has the shape (batch_size,).

**class gluon.loss.L1Loss(weight=None, batch_axis=0, **kwargs)**

   Calculates the mean absolute error between output and label.

      L = \frac{1}{2}\sum_i \vert {output}_i - {label}_i \vert.

   Output and label must have the same shape.

   :Parameters:
      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   Input shape:
      *output* is the prediction tensor. *label* is the truth tensor
      and should have the same shape as *output*. *sample_weight* is a
      matrix for per-sample weighting. Must be broadcastable to the
      same shape as loss. For example, if loss has shape (64, 10) and
      you want to weigh each sample in the batch separately,
      *sample_weight* should have shape (64, 1).

   Output shape:
      The loss output has the shape (batch_size,).

**class
gluon.loss.SigmoidBinaryCrossEntropyLoss(from_sigmoid=False,
weight=None, batch_axis=0, **kwargs)**

   The cross-entropy loss for binary classification. (alias:
   SigmoidBCELoss)

   BCE loss is useful when training logistic regression.

      loss(o, t) = - 1/n \sum_i (t[i] * log(o[i]) + (1 - t[i]) * log(1
      - o[i]))

   :Parameters:
      * **from_sigmoid** (bool, default is *False*) -- Whether the
        input is from the output of sigmoid. Set this to false will
        make the loss calculate sigmoid and then BCE, which is more
        numerically stable through log-sum-exp trick.

      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   Input shape:
      *output* is the prediction tensor. *label* is the truth tensor
      and should have the same shape as *output*. *sample_weight* is a
      matrix for per-sample weighting. Must be broadcastable to the
      same shape as loss. For example, if loss has shape (64, 10) and
      you want to weigh each sample in the batch separately,
      *sample_weight* should have shape (64, 1).

   Output shape:
      The loss output has the shape (batch_size,).

``gluon.loss.SigmoidBCELoss``

   alias of ``SigmoidBinaryCrossEntropyLoss``

**class gluon.loss.SoftmaxCrossEntropyLoss(axis=-1,
sparse_label=True, from_logits=False, weight=None, batch_axis=0,
**kwargs)**

   Computes the softmax cross entropy loss. (alias: SoftmaxCELoss)

   If *sparse_label* is *True*, label should contain integer category
   indicators:

      p = {softmax}({output})  L = -\sum_i {log}(p_{i,{label}_i})

   Label's shape should be output's shape without the *axis*
   dimension. i.e. for *output.shape* = (1,2,3,4) and axis = 2,
   *label.shape* should be (1,2,4).

   If *sparse_label* is *False*, label should contain probability
   distribution with the same shape as output:

      p = {softmax}({output})  L = -\sum_i \sum_j {label}_j
      {log}(p_{ij})

   :Parameters:
      * **axis** (*int*, *default -1*) -- The axis to sum over when
        computing softmax and entropy.

      * **sparse_label** (*bool*, *default True*) -- Whether label
        is an integer array instead of probability distribution.

      * **from_logits** (*bool*, *default False*) -- Whether input
        is a log probability (usually from log_softmax) instead of
        unnormalized numbers.

      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   Input shape:
      *output* is the prediction tensor. The batch axis and softmax
      axis should be consistent with the value used in the
      constructor. *label* is the truth tensor. When *sparse_label* is
      true, *label* should have one less dimension (axis) than
      *output* tensor. Otherwise, the shape of *label* must be the
      same as output. For example, when *sparse_label* is true, if
      *output* has shape *(batch_size, x1, x2, c)* and axis is *-1*,
      *label* should have shape *(batch_size, x1, x2)*. If
      *sparse_label* is false, *label* should have shape *(batch_size,
      x1, x2, c)*. *sample_weight* is a matrix for per-sample
      weighting. Must be broadcastable to the same shape as loss. For
      example, if loss has shape (64, 10) and you want to weigh each
      sample in the batch separately, *sample_weight* should have
      shape (64, 1).

   Output shape:
      The loss output has the shape (batch_size,).

``gluon.loss.SoftmaxCELoss``

   alias of ``SoftmaxCrossEntropyLoss``

**class gluon.loss.KLDivLoss(from_logits=True, weight=None,
batch_axis=0, **kwargs)**

   The Kullback-Leibler divergence loss.

   KL divergence is a useful distance measure for continuous
   distributions and is often useful when performing direct regression
   over the space of (discretely sampled) continuous output
   distributions.

      L = 1/n \sum_i (label_i * (log(label_i) - output_i))

   Label's shape should be the same as output's.

   :Parameters:
      * **from_logits** (bool, default is *True*) -- Whether the input
        is log probability (usually from log_softmax) instead of
        unnormalized numbers.

      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

      * **batch_axis** (*int*, *default 0*) -- The axis that
        represents mini-batch.

   Input shape:
      *output* is the prediction tensor. *label* is the truth tensor
      and should have the same shape as *output*. *sample_weight* is a
      matrix for per-sample weighting. Must be broadcastable to the
      same shape as loss. For example, if loss has shape (64, 10) and
      you want to weigh each sample in the batch separately,
      *sample_weight* should have shape (64, 1).

   Output shape:
      The loss output has the shape (batch_size,).

**class gluon.loss.CTCLoss(layout='NTC', label_layout='NT',
weight=None, **kwargs)**

   Connectionist Temporal Classification Loss.

   See "Connectionist Temporal Classification: Labelling Unsegmented
   Sequence Data with Recurrent Neural Networks" paper for more
   information.

   :Parameters:
      * **layout** (*str*, *default 'NTC'*) -- Layout of the output
        sequence activation vector.

      * **label_layout** (*str*, *default 'NT'*) -- Layout of the
        labels.

      * **weight** (*float* or *None*) -- Global scalar weight for
        loss.

   Input shape:
      *data* is an activation tensor (i.e. before softmax). Its shape
      depends on *layout*. For *layout='TNC'*, this input has shape
      *(sequence_length, batch_size, alphabet_size)* Note that the
      last dimension with index *alphabet_size-1* is reserved for
      special blank character.

      *label* is the label index matrix with zero-indexed labels. Its
      shape depends on *label_layout*. For *label_layout='TN'*, this
      input has shape *(label_sequence_length, batch_size)*. Padding
      mask of value ``-1`` is available for dealing with unaligned
      label lengths. When *label_lengths* is specified, label lengths
      are directly used and padding mask is not allowed in the label.
      When *label_lengths* is not specified, the first occurrence of
      ``-1`` in each sample marks the end of the label sequence of
      that sample.

      For example, suppose the vocabulary is *[a, b, c]*, and in one
      batch we have three sequences 'ba', 'cbb', and 'abac'. We can
      index the labels as *{'a': 0, 'b': 1, 'c': 2}*. The alphabet
      size should be 4, and we reserve the channel index 3 for blank
      label in data tensor. The padding mask value for extra length is
      -1, so the resulting *label* tensor should be padded to be:

      ::

         [[1, 0, -1, -1], [2, 1, 1, -1], [0, 1, 0, 2]]

      *data_lengths* is optional and defaults to None. When specified,
      it represents the actual lengths of data. The shape should be
      (batch_size,). If None, the data lengths are treated as being
      equal to the max sequence length. This should be used as the
      third argument when calling this loss.

      *label_lengths* is optional and defaults to None. When
      specified, it represents the actual lengths of labels. The shape
      should be (batch_size,). If None, the label lengths are derived
      from the first occurrence of the value specified by
      *padding_mask*. This should be used as the fourth argument when
      calling this loss.

   Output shape:
      The CTC loss output has the shape (batch_size,).
