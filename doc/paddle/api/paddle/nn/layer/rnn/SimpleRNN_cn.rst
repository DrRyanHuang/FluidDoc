.. _cn_api_paddle_nn_layer_rnn_SimpleRNN:

SimpleRNN
-------------------------------

.. py:class:: paddle.nn.SimpleRNN(input_size, hidden_size, num_layers=1, activation="tanh", direction="forward", dropout=0., time_major=False, weight_ih_attr=None, weight_hh_attr=None, bias_ih_attr=None, bias_hh_attr=None)



**简单循环神经网络**

该OP是简单循环神经网络（SimpleRNN），根据输出序列和给定的初始状态计算返回输出序列和最终状态。在该网络中的每一层对应输入的step，每个step根据当前时刻输入x（t）和上一时刻状态h（t-1）计算当前时刻输出y（t）并更新状态h（t）。

状态更新公式如下：

..  math::

        h_{t} & = \mathrm{tanh}(W_{ih}x_{t} + b_{ih} + W_{hh}h_{t-1} + b_{hh})

        y_{t} & = h_{t}

参数：
    - **input_size** (int) - 输入的大小。
    - **hidden_size** (int) - 隐藏状态大小。
    - **num_layers** (int，可选) - 网络层数。默认为1。
    - **direction** (str，可选) - 网络迭代方向，可设置为forward，backward或bidirectional。默认为forward。
    - **time_major** (bool，可选) - 指定input的第一个维度是否是time steps。默认为False。
    - **dropout** (float，可选) - dropout概率，指的是出第一层外每层输入时的dropout概率。默认为0。
    - **activation** (str, 可选) - 网络中每个单元的激活函数。可以是tanh或relu。默认为tanh。
    - **weight_ih_attr** (ParamAttr，可选) - weight_ih的参数。默认为None。
    - **weight_hh_attr** (ParamAttr，可选) - weight_hh的参数。默认为None。
    - **bias_ih_attr** (ParamAttr，可选) - bias_ih的参数。默认为None。
    - **bias_hh_attr** (ParamAttr，可选) - bias_hh的参数。默认为None。
    
输入:
    - **inputs** (Tensor) - 网络输入。如果time_major为False，则Tensor的形状为[batch_size,time_steps,input_size]，如果time_major为True，则Tensor的形状为[time_steps,batch_size,input_size]。
    - **initial_states** (Tensor，可选) - 网络的初始状态，形状为[num_lauers * num_directions, batch_size, hidden_size]。如果没有给出则会以全零初始化。
    - **sequence_length** (Tensor，可选) - 指定输入序列的长度，形状为[batch_size]，数据类型为int64或int32。在输入序列中所有time step不小于sequence_length的元素都会被当作填充元素处理（状态不再更新）。

输出:
    - **outputs** (Tensor) - 输出，由前向和后向cell的输出拼接得到。如果time_major为False，则Tensor的形状为[batch_size,time_steps,num_directions * hidden_size]，如果time_major为True，则Tensor的形状为[time_steps,batch_size,num_directions * hidden_size]，当direction设置为bidirectional时，num_directions等于2，否则等于1。
    - **final_states** (Tensor) - 最终状态。形状为[num_lauers * num_directions, batch_size, hidden_size],当direction设置为bidirectional时，num_directions等于2，否则等于1。

**代码示例**：

.. code-block:: python

            import paddle

            rnn = paddle.nn.SimpleRNN(16, 32, 2)
            
            x = paddle.randn((4, 23, 16))
            prev_h = paddle.randn((2, 4, 32))
            y, h = rnn(x, prev_h)
            
            print(y.shape)
            print(h.shape)
            
            #[4,23,32]
            #[2,4,32]
