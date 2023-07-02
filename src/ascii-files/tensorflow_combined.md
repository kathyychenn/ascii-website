

== ./tensorflow/tensorflow_0.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/cc/framework/gradients_test.cc#L62-L85

```c
// Example:
< ASCII >
//      ^             ^
//    dy|           dx|        (MatMul Gradient Graph)
//      |             |
//   MatMul_1      MatMul_2
//   ^   ^          ^    ^
//   |   |----------|    |
//   |        ^          |
//   |      dz|          |
//   |        |          |
//   |     Const_3       |
//   |                   |
//   |        ^          |
//   |       z|          |     (MatMul Forward Graph)
//   |        |          |
//   |      MatMul_0     |
//   |     /        \    |
//   |    ^          ^   |
//   |    |          |   |
//   |---x|         y|---|
//        |          |
//        |          |
//      Const_0   Const_1
< ASCII >
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_1.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/cc/ops/while_loop.cc#L148-L171

```c
// A while loop with a single loop variable looks like this:
//
< ASCII >
// (output)
//     ^    +---------------+
//     |    | body subgraph +-------------+
//    Exit  +---------------+             |
//      ^    ^                            |
//      |    |                            |
//      Switch<--------+                  v
//        ^            |             NextIteration
//        |     +------+--------+         |
//        +---->| cond subgraph |         |
//        |     +---------------+         |
//       Merge<---------------------------+
//       ^
//       |
//    Enter
//      ^
//      |
//   (input)
< ASCII >
//
// If there are multiple loop variables, each of the control flow ops is
// duplicated for each loop variable.
// TODO(skyewm): link to public version of design doc
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_10.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/experimental/tac/transforms/cost_model.h#L42-L50

```c
// Get the transfer cost given from & to hardware info.
// We will only calculate for the "necessary" tensor transferred.
// from_graph & to_graph are used to compute the "necessary" tensors.
< ASCII >
//     from_graph
//    /    \   \
//  out1   out2  out3
//           \   /
//           to_graph
< ASCII >
// So only out2 & out3 are counted.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_100.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L162-L168

```c
< ASCII >
//     input1
//       |
//     slice
//       |
//      cast
//       |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_101.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L171-L179

```c
< ASCII >
//       input
//         |
//      Reshape
//       /   \
//     Add   Add (Optional)
//       \   /
//        Mul
//         |
//       output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_102.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L183-L189

```c
< ASCII >
//    input
//      |
//   concat
//      |
//   cosinus
//      |
//   output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_103.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/model_builder.cc#L1350-L1377

```c
// Basic LSTM Cell:
//
//  1name = name is at input  index 1
//  name1 = name is at output index 1
//
< ASCII >
//    0input     1prev_activ
//       \        /
//        [[concat]]
//             \
//       concat_temp2  2weights  3biases
//              \      /        /
//             [[fully-connected]]
//               \
//         activ_temp3    4prev_state
//                 \      /
//                 [[LSTM]]
//                 /      \
//           new_state1    activation0
//
< ASCII >
// For full LSTM cells, see this blog post:
// https://colah.github.io/posts/2015-08-Understanding-LSTMs/
// In addition to Peephole connections and Combined Input Forget Gates (CIFG)
// described in that post, this code also adds the following optional features:
// - Configurable activations (sigmoid or TANH)
// - L2 Normalization of gates: https://arxiv.org/abs/1607.06450
// - Output projection:
//     https://www.isca-speech.org/archive/interspeech_2014/i14_0338.html
// - Configurable clipping of cell state and output state.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_104.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/task/gpu_operation.cc#L729-L735

```c
< ASCII >
//      input                input           input
//     /    \               /    \             |
//  elem0  elem1           |    elem1          |
//     \    /      -->      \    /      -->  elem
//   elem_root              elem2              |
//       |                    |                |
//     output               output           output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_105.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/task/gpu_operation.h#L232-L238

```c
< ASCII >
//    input       input
//      |           |
//    elem0         |
//      |    -->  elem
//    elem1         |
//      |           |
//    output      output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_106.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/task/gpu_operation.h#L244-L250

```c
< ASCII >
//      input           input
//     /    \             |
//  elem0    |            |
//     \    /      -->  elem
//     elem1              |
//       |                |
//     output           output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_107.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/task/gpu_operation.h#L256-L262

```c
< ASCII >
//      input           input
//     /    \             |
//    |    elem0          |
//     \    /      -->  elem
//     elem1              |
//       |                |
//     output           output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_108.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/task/gpu_operation.h#L268-L274

```c
< ASCII >
//      input           input
//     /    \             |
//  elem0  elem1          |
//     \    /      -->  elem
//   elem_root            |
//       |                |
//     output           output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_109.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/tasks/mean_stddev_normalization.h#L73-L92

```c
// MeanStdDevNormalization fusion works with this subgraph
< ASCII >
//       input
//       /    \
//      |    mean
//       \    /
//     substraction
//       /    \
//      |      |
//      |    square
//      |      |
//      |     mean
//      |      |
//      |     add
//      |      |
//      |    rsqrt
//      |      |
//       \    /
//    multiplication
//          |
//        output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_11.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/experimental/tac/transforms/device_transform_patterns.cc#L440-L470

```c
// TFL fully_connected basically does:
// Weight * Input + bias.
// Input layout is : [..., depth]
// Weight layout is : [output, depth]
// Bias is [output].
//
// While conv2d is:
// Filter: [NHWC]
// Input is also: [NHWC]
// Bias is [N]
//
// So to perform the transform, we need to insert a few reshape ops:
//
< ASCII >
//  Input   weight   bias
//   \      /      /
//       FC
//       |
//     output
//
//     |
//    \/
//
//  Input   weight
//   |        |
//  Reshape  Reshape  bias
//  |         |      /
//     conv
//      |
//     reshape
//      |
//    output
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_110.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/transformations/add_quant_adjustments_test.cc#L85-L92

```c
// Scenario:
< ASCII >
// -> Add -> QuantizeAndDequantize -> Add ->
//        |                            ^
//        |                            |
//        ------------------------------
< ASCII >
//
// A new QuantizeAndDequantize should only be added after the left/first 'Add'
// op, and it should connect to both its consumers.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_111.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/gl/compiler/fuse_auto_input.h#L26-L39

```c
// Fuses nodes that have auto output with auto input node using the following
// rules.
//
< ASCII >
// Source graph:
//   A B C
//   \ | /
//     D
< ASCII >
//
// - A, B and C each have a single output marked as AUTO
// - Each output is used only by D
// - D has all inputs marked as AUTO
//
// Result: in the best case a single node that does (A,B,C)+D operations.
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_112.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/gl/kernels/lstm.cc#L32-L44

```c
// Basic LSTMCell gates.
//
< ASCII >
//  inputs:  0             1
//           activ_temp    prev_state
//                  \      /
//               [[LSTM gates]]
//                  /      \
//            new_state    activation
//  outputs:  0            1
< ASCII >
//
// The size of activ_temp should be 4x size of new_state.
// The size of prev_state == new_state == activation.
//
```
## Visual type:
- #custom


== ./tensorflow/tensorflow_113.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/nnapi/nnapi_delegate_device_selection_test.cc#L364-L372

```c
// This is a model with two ops:
//
< ASCII >
//  input1 ---->
//                ADD --
//  input2   -->        |
//                       -->
//                          SUB --> output
//  input3 ---------------->
< ASCII >
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_114.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/nnapi/nnapi_delegate_nnapi_failure_handling_test.cc#L45-L53

```c
// This is a model with two ops:
//
< ASCII >
//  input1 ---->
//                ADD --
//  input2   -->        |
//                       -->
//                          SUB --> output
//  input3 ---------------->
< ASCII >
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_115.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/experimental/acceleration/configuration/nnapi_plugin_test.cc#L220-L230

```c
/*
 * Building a model with three operations that can be used to create multiple
 * delegated partitions.
 *
< ASCII >
 *  input1 ---
 *            | -  ADD -- ROUND --
 *            |                   | - ADD -- output1
 *  input2 ---                    |
 *                                |
 *  input3 -----------------------
< ASCII >
 */
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_116.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/experimental/acceleration/mini_benchmark/model_modifier/custom_validation_embedder.h#L29-L52

```c
// Create a model with custom validation graph.
//
// 'validation model' (new subgraph)
< ASCII >
// input (batch_size)
//           |
// +-----------------------+
// |'main_model' (0)       |
// | +---------------+     |
// | |input          +---+ |
// | +---------------+   | |
// |                     ~ |
// | +---------------+   | |
// | |outputs        +<--+ |
// | +---------------+     |
// |                       |
// +-----------------------+
//           |
// output (batch_size)
< ASCII >
//
// The new model contains all the information from main_model, with an extra
// subgraph for validation purposes. The validation graph calls the primary
// subgraph with batch_size. The input data is embedded to the validation graph.
// custom_input should have the same order as the input in the main_model. E.g.
// custom_input[i] will be mapped to main_model.input[i].
```
## Visual type:
- #custom


== ./tensorflow/tensorflow_117.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/experimental/acceleration/mini_benchmark/model_modifier/validation_graph_builder.h#L36-L90

```c
// Class for building the validation entry-point graph that calls into the main
// graph and a metrics graph. Like this (boxes are tensors with plural names
// meaning possibly multiple tensors, arrows are ops and numbers in parentheses
// are subgraph indices):
< ASCII >
// +--------------------------------------+
// | Graph created by this class (1)      |
// |                                      |
// | +-----------input-+                  |
// | |jpeg input       |                  |
// | +-----+-----------+                  |
// |       |                              |
// |       | decode                       |
// |       v                              |
// | +-----+-----------+                  |
// | |quantized image  |                  |
// | +-----+-----------+                  |  +-----------------------+
// |       |                              |  |'main_model' (0)       |
// |       | dequantize (optional)        |  | +---------------+     |
// |       v                              |  | |input          +---+ |
// | +-----+-----------+                  |  | +---------------+   | |
// | |float image      |                  |  |                     ~ |
// | +-----+-----------+                  |  | +---------------+   | |
// |       |  call                        |  | |outputs        +<--+ |
// |       +<------------------------------->+ +---------------+     |
// |       v                              |  |                       |
// | +-----+-----output+ +---------input+ |  +-----------------------+
// | |actual outputs   | |golden outputs| |
// | +-----+-----------+ +-----------+--+ |
// |       |                         |    |
// |       | dequantize (optional)   |    |
// |       |                         |    |
// | +-----+-------------------------+-+  |
// | | dequantized actual and golden   |  |
// | | outputs (validation inputs)     |  |
// | +-----+---------------------------+  |  +-----------------------+
// |       |  call                        |  |'validation model' (2) |
// |       +<------------------------------->+                       |
// |       v                              |  | +---------------+     |
// | +-----+-----output+                  |  | |inputs         +---+ |
// | |results          |                  |  | +---------------+   | |
// | +-----------------+                  |  |                     ~ |
// |                                      |  | +---------------+   | |
// |                                      |  | |outputs        +<--+ |
// |                                      |  | +---------------+     |
// |                                      |  |                       |
// +--------------------------------------+  +-----------------------+
< ASCII >
//
// It's important the 'main_model' has subgraph index 0 so that it is used as
// the primary subgraph by the TFLite interpreter. The other indices are
// arbitrary.
// TODO(b/172541832): Handle a main model with more than one subgraph.
//
// Note that the jpeg input is marked as an input in this graph, as TFLite
// graphs must have inputs. However, it will be pre-filled from the jpeg_data
// and doesn't need to be filled by the user of the model.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_118.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/kernels/cpu_backend_gemm.h#L47-L51

```c
//  On x86 platforms:
< ASCII >
//  (default)         |      gemmlowp   |     Ruy        | eigen |
//  TFLITE_X86_RUY_\  |      Ruy        |     Ruy        | Ruy   |
//  ENABLED && (AVX
//  or above available)
< ASCII >
```
## Visual type:
- #ignore


== ./tensorflow/tensorflow_119.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/kernels/internal/quantization_util.cc#L28-L41

```c
// These constants are used to manipulate the binary representation of doubles.
// Double-precision binary64 floating point format is:
< ASCII >
// Bit |  63  |  62-52   |   51-0   |
//     | Sign | Exponent | Fraction |
// To avoid 64-bit integers as much as possible, I break this into high and
// low 32-bit chunks. High is:
// Bit |  31  |  30-20   |      19-0     |
//     | Sign | Exponent | High Fraction |
// Low is:
// Bit |     31-0     |
//     | Low Fraction |
< ASCII >
// We then access the components through logical bit-wise operations to
// extract the parts needed, with the positions and masks derived from the
// layout shown above.
```
## Visual type:
- #memory-layout


== ./tensorflow/tensorflow_12.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/experimental/tac/transforms/pick_subgraphs.cc#L50-L80

```c
// This pass is used to "determine" the best combinations of the whole "graph".
//
< ASCII >
// Assume we have the graph looks like below:
//    subgraph1 (CPU/GPU)    subgraph2 (CPU)
//      \                     /
//      subgraph3 (CPU/GPU)     subgraph4 (CPU/GPU)
//         |                  /
//      subgraph5 (CPU/GPU)
//         |
//      subgraph6 (CPU)
< ASCII >
//
//  We want to evaluate the possible options and minize the overall costs to
// produce a graph like below:
//
< ASCII >
//    subgraph1 (GPU)   subgraph2(CPU)
//       \              /
//     subgraph3 (GPU)      subgraph4(GPU)
//         |             /
//      subgraph5 (GPU)
//         |
//      subgraph6 (CPU)
< ASCII >
//
// The overall workflow of the pick subgraphs pass:
//  1) Build subgraphs
//    1.1) Collect output subgraphs.
//    1.2) Build `Subgraph` and their "alternative view" from FuncOp.
//  2) Pick subgraphs
//    2.1) Populate the "dp table" for (subgraph, hardware).
//    2.2) Make decisions based on the populated dp table.
//    2.3) Rewire the whole graph based on the desicions.
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_120.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/kernels/internal/quantization_util_test.cc#L155-L161

```c
// Example taken from http://www.tensorflow.org/performance/quantization
//
< ASCII >
//  Quantized | Float
//  --------- | -----
//  0         | -10.0
//  255       | 30.0
//  128       | 10.0
< ASCII >
```
## Visual type:
- #table

== ./tensorflow/tensorflow_121.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/tsl/platform/ctstring.h#L101-L109

```c
// Copy/Move/Assign semantics
//
< ASCII >
//        | src     | dst          | complexity
// Copy   | *       |  SMALL/LARGE | fixed/O(size)
// Assign | SMALL   |  SMALL       | fixed
// Assign | OFFSET  |  VIEW        | fixed
// Assign | VIEW    |  VIEW        | fixed
// Assign | LARGE   |  LARGE       | O(size)
// Move   | *       |  same as src | fixed
< ASCII >
```
## Visual type:
- #table


== ./tensorflow/tensorflow_13.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/experimental/tac/transforms/pick_subgraphs.cc#L188-L219

```c
// Here we choose to do a greedy dynamic programming based algorithm for
// simplicity.
//
// See the following graph:
//
< ASCII >
//    input_subgraph_1      ....      input_subgraph_n
//              \                          /
//               \                        /
//                   current_subgraph
//                      /     |      \
< ASCII >
//
// Assume all the input subgraphs of the current subgraph are independent.
// If we already got optimal results for all the input subgraphs.
// Then the current_subgraph's aggregated optimal costs with regards to target
// perspective is simply:
//     for target in current_subgraph.supported_targets:
//       total_cost = 0
//       for input_subgraph in current_subgraph.input_subgraphs:
//         input_cost = kInfinity
//         for input_target in input_subgraphs.upported_targets:
//           # cost = aggregated cost for input_subgraph with transfer cost.
//           input_cost = min(input_cost, cost)
//         total_cost += input_cost
//       total_cost += current_subgraph.get_computation_cost(target)
//
// Note: for input subgraphs are not independent case, the dp case it a little
// bit complicated to handle. A potential thought is resolve only where
// conflict "happened".
//
// The above mentioned thought should probably be revisited for better thought
// or expanded more for more careful design.
// TODO(renjieliu): We may revisit this later.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_14.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/ir/tfl_ops.cc#L1940-L1955

```c
// Remove redundant unpack pack op.
// If a unpack op is followed by a pack op, we can remove the pack op, if the
// unpack op is only consumed by the pack op, it will be removed as well.
// An example illustration is:
< ASCII >
//                  Unpack [5, 8, 9], axis = 1
//                /       \
//            value  ...  value [5, 9], 8 values in total
//              \           /
//                 pack,   axis = 1
//                   |
//               value   [5, 8, 9]
//
//   This can actually be simplified into just:
//
//           =>   Value [5, 8, 9]
< ASCII >
// TODO(b/133341698): Move to tablegen when variadic is supported.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_15.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/quantization/tensorflow/tf_to_quant.cc#L59-L83

```c
// Inserts a "tfl.quantize" and "tfl.dequantize" op pair (QDQs) after the
// "tf.FakeQuantWithMinMaxVarsOp" to be constant folded. Since the constant
// folding logic will use a "arith.constant" op to replace the
// "tf.FakeQuantWithMinMaxVarsOp", the "tfl.quantize" op is used to preserve
// the quantization parameters as a TypeAttr and "tfl.dequantize" op used to
// convert the output type to the next op. Here are the transformations:
//
< ASCII >
// input   min cst       max cst          input   min cst       max cst
//  \       |             |                \       |             |
//   \  (tf.Identity) (tf.Identity)   =>    \  (tf.Identity) (tf.Identity)
//    \     |             |                  \     |             |
//       tf.FakeQuantWithMinMaxVars       tf.FakeQuantWithMinMaxVars
//                   |                                 |
//                                                tf.quantize
//                                                     |
//                                                tf.dequantize
//                                                     |
// If the input is a constant, the result pattern will eventually converted to
//
//            quant-emulated input
//                   |
//               tf.quantize
//                   |
//              tf.dequantize
//                   |
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_16.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/transforms/prepare_tf.cc#L1234-L1250

```c
// Convert rfft to rfft2d.
// The transformation pattern looks like below:
//
< ASCII >
//    input     fft_len
//     \      /
//     rfft
//
//     ||
//     \/
//
//   input       fft_len
//    \            /
//   expand_dim    concat with [1] at the front
//      \         /
//     rfft_2d
//       |
//     squeeze
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_17.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/utils/fake_quant_utils.h#L61-L95

```c
// Inserts a "tfl.quantize" and "tfl.dequantize" op pair (QDQs) after the
// tf.FakeQyantWithMinMax{Vars|VarsPerChannel|Args}Op
// before the op being constant folded. Since the constant
// folding logic will use a "arith.constant" op to replace the
// "tf.FakeQuantWithMinMaxVarsOp", the "tfl.quantize" op is used to preserve
// the quantization parameters as a TypeAttr and "tfl.dequantize" op used to
// convert the output type to the next op. Here are the transformations:
//
< ASCII >
// input   min cst       max cst          input   min cst       max cst
//  \       |             |                \       |             |
//   \  (tf.Identity) (tf.Identity)   =>    \  (tf.Identity) (tf.Identity)
//    \     |             |                  \     |             |
//       tf.FakeQuantWithMinMaxVars       tf.FakeQuantWithMinMaxVars
//                   |                                 |
//                                                tfl.quantize
//                                                     |
//                                                tfl.dequantize
//                                                     |
// If the input is a constant, the result pattern will eventually converted to
//
//            quant-emulated input
//                   |
//               tfl.quantize
//                   |
//              tfl.dequantize
//                   |
< ASCII >
//
//
// Warns if the (most likely unwanted, currently not quite correctly handled)
// case of back-to-back tf.FakeQuant occurs
//
< ASCII >
//             tf.FakeQuant*
//                   |
//             tf.FakeQuant*
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_18.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/quantization/tensorflow/utils/fake_quant_utils.h#L68-L89

```c
// Inserts a "quant.qcast" and "quant.dcast" op pair (QDQs) in place of the
// tf.FakeQyantWithMinMax{Vars|VarsPerChannel|Args}Op
// before the op being constant folded. Since the constant
// folding logic will use a "arith.constant" op to replace the
// "tf.FakeQuantWithMinMaxVarsOp", the "quant.qcast" op is used to preserve
// the quantization parameters as a TypeAttr and "quant.dcast" op used to
// convert the output type to the next op. Here are the transformations:
//
< ASCII >
// input   min cst       max cst              input
//  \       |             |                     |
//   \  (tf.Identity) (tf.Identity)   =>   quant.qcast
//    \     |             |                     |
//       tf.FakeQuantWithMinMaxVars        quant.dcast
//                   |                          |
< ASCII >
//
// Warns if the (most likely unwanted, currently not quite correctly handled)
// case of back-to-back tf.FakeQuant occurs
//
< ASCII >
//             tf.FakeQuant*
//                   |
//             tf.FakeQuant*
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_19.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/legalize_hlo.cc#L1501-L1510

```c
// Matches %iota generated from the following mlir codes (rank 2 example):
//
< ASCII >
// %iota = mhlo.constant dense<[[0, 1, 2, ..., L],
//                              [0, 1, 2, ..., L]
//                              ...
//                              [0, 1, 2, ..., L]]>,
// where $dimensions is of size 1.
< ASCII >
//
// StridedArrayViews are used to check the iota property across the constant
// data so that the iota dimension does not need to be the (inner) z-dimension.
```
## Visual type:
- #matrix

== ./tensorflow/tensorflow_2.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/cluster_scoping_pass.cc#L67-L86

```c
// NB! We append a new scope as suffix to the _XlaInternalScope attribute
// instead of overriding the old value.  In other words, appending scope B to
// scope A creates the conjunction of the scopes A and B (i.e, A & B) and,
// in effect, the node gets both the old and new scopes.  As a unique scope
// disallows a node being merged with nodes in other scopes, the scope
// conjunction preserves the semantic of the old scope (i.e., the node still
// cannot be merged with the previously incompatible nodes.)
//
// For example, the below case should be rare in practice but can serve for the
// purpose of discussion.  After adding scopes for both Stage and Unstage,
// Node_Y will receive both scopes "unstage" and "stage", while Node_X receives
// only scope "stage".  The semantic of scope "unstage" is preserved although
// scope "stage" is later appended.  As a result, Node_X and Node_Y will be put
// into different clusters.
//
< ASCII >
//                Unstage -> Node_Y (scope "unstage & stage")
//                              |
//                              V
//  Node_X (scope "stage") -> Stage
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_20.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/legalize_hlo_patterns.td#L422-L446

```c
// Converts a dag of HLOs representing floor_div to tf.FloorDiv.
// The pattern matched executes the following computation:
//
// rem = remainder(arg0, arg1)
// for i in 0 to len(arg1):
//    rem[i] = arg0[i] - rem[i] / arg1[i]
//    if (rem[i] != 0 && sign(arg1[i]) != sign(rem[i]))
//       rem[i] -= 1.0
// return round_nearest_afz(rem)
// As a dag this looks like the following:
< ASCII >
//                       round
//                         |
//              -------- select
//              |          |    \
//             &&          +    div
//           /   |        / \
//        !=    !=       div -1
//       / |   / |      / |
//    rem 0.0 sn sn1    -  $1
//   / |      |  |    / |
// $0 $1     $1 rem  $0 rem
< ASCII >
// Note that named operators like 'sn' and 'sn1' are different values produced by
// the same function in this case the sign function. Named values like 'div'
// refer to the same value produced by the same function, in this case division.
// Mathematical symbols do not indicate a re-use of the value.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_21.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/legalize_hlo_patterns.td#L475-L502

```c
// Converts a dag of HLOs representing floor_div with a splat constant to
// tf.FloorDiv. The pattern matched executes the following computation:
// This particular pattern matches multiplication with the reciprocal of the
// constant instead of dividing by the constant.
// rem = remainder(arg0, cst)
// for i in 0 to len(arg0):
//    rem[i] = (arg0[i] - rem[i]) * 1 / cst
//    if (rem[i] != 0 && sign(cst) != sign(rem[i]))
//       rem[i] += -1.0
// return round_nearest_afz(rem)
// As a dag this looks like the following:
< ASCII >
//                       round
//                         |
//              -------- select
//              |           |     \
//             &&           +     mul
//           /   |         / \
//        !=    !=        mul -1
//       / |   /  |       / |
//    rem 0.0 cs1 sn1   -  cs2
//   / |           |   / |
// $0 cst         rem $0 rem
< ASCII >
// cs1 == sign(cst)
// cs2 = 1 / cst i.e. the reciprocal
// Note that named operators like 'sn' and 'sn1' are different values produced by
// the same function in this case the sign function. Named values like 'div'
// refer to the same value produced by the same function, in this case division.
// Mathematical symbols do not indicate a re-use of the value.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_22.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/legalize_hlo_patterns.td#L533-L560

```c
// Converts a dag of HLOs representing floor_div with a splat constant to
// tf.FloorDiv. The pattern matched executes the following computation:
// This particular pattern matches division with the constant.
// .
// rem = remainder(arg0, cst)
// for i in 0 to len(arg0):
//    rem[i] = (arg0[i] - rem[i]) / cst
//    if (rem[i] != 0 && sign(cst) != sign(rem[i]))
//       rem[i] -= 1.0
// return round_nearest_afz(rem)
// As a dag this looks like the following:
< ASCII >
//                       round
//                         |
//              -------- select
//              |           |     \
//             &&           +     div
//           /   |         / \
//        !=    !=        div -1
//       / |   /  |       / |
//    rem 0.0 cs1 sn1   -  cs2
//   / |           |   / |
// $0 cst         rem $0 rem
// cs1 == sign(cst)
// cs2 = 1 / cst i.e. the reciprocal
< ASCII >
// Note that named operators like 'sn' and 'sn1' are different values produced by
// the same function in this case the sign function. Named values like 'div'
// refer to the same value produced by the same function, in this case division.
// Mathematical symbols do not indicate a re-use of the value.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_23.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/legalize_hlo_patterns.td#L591-L622

```c
// Converts a dag of HLOs representing floor_div with a broadcasted vector
// constant to tf.FloorDiv. The pattern matched executes the following
// computation:
// scs = sign(cst)
// bcst = broadcast(cst)
// rem = remainder(arg0, bcst)
// for i in 0 to len(arg0):
//    rem[i] = arg0[i] - rem[i] * / bcst
//    if (rem[i] != 0 && scs != sign(rem[i]))
//       rem[i] -= 1.0
// return round_nearest_afz(rem)
// Where scs is a splat constant folded sign on the unbroadcasted tensor.
//
// As a dag this looks like the following:
< ASCII >
//                       round
//                         |
//              -------- select
//              |           |     \
//             &&           +     div
//           /   |         / \
//        !=    !=        div -1
//       / |   /  |       / |
//    rem 0.0 scs sn1   -  bcst
//   / |           |   / |
// $0 bcst        rem $0 rem
//     |
//    cst
// scs == sign(cst) == sign(bcst)
< ASCII >
// Note that named operators like 'sn' and 'sn1' are different values produced by
// the same function in this case the sign function. Named values like 'div'
// refer to the same value produced by the same function, in this case division.
// Mathematical symbols do not indicate a re-use of the value.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_24.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/lower_tf.cc#L210-L248

```c
// Lowers AddN op to a sequence of AddV2 ops to accumulate operands.
//
// Note that to improve the parallelism, AddN op uses tree-based reduction.
// For example, tf.AddN([0, 1, 2, 3, 4]) behaves as follows:
//
< ASCII >
//                 0     1     2     3     4
//                 |     |     |     |     |
//                 -------     -------     |
//                    |           |        |
//                    5           6        |
//                    |           |        |
//                    -------------        |
//                          |              |
//                          7              |
//                          |              |
//                          ----------------
//                                 |
//                                 8
< ASCII >
//
// Example:
//
//   %result = "tf.AddN"(%0, %1, %2)
//
// is lowered to:
//
//   %sum0 = "tf.AddV2"(%0, %1)
//   %result = "tf.AddV2"(%sum0, %2)
//
// While
//
//   %result = "tf.AddN"(%0, %1, %2, %3, %4)
//
// is lowered to:
//
//   %sum0 = "tf.AddV2"(%0, %1)
//   %sum1 = "tf.AddV2"(%2, %3)
//   %sum2 = "tf.AddV2"(%sum0, %sum1)
//   %result = "tf.AddV2"(%sum2, %4)
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_25.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/merge_control_flow.cc#L218-L229

```c
// Return the iterator of the IfRegion Op. This is the last IfRegion
// in the segment.
// For example, we have the following sequence of IfRegions
< ASCII >
// `````
//      1          2          3         4           5
// IfRegionA, IfRegionA, IfRegionA, IfRegionA, IfRegionA
// `````
< ASCII >
// The first three IfRegionA are in one group and the last two are in another
// group. Then when we call FindLastIfInSegment for the first segment, it
// will return iterator of the 3rd IfRegionA.
// In the same way, when we call it for the second segment, it will return
// iterator of the 5th IfRegionA.
```
## Visual type:
- #sequence

== ./tensorflow/tensorflow_26.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/optimize.td#L68-L79

```c
// This rule does the following pattern match and rewrite:
//
< ASCII >
//       input     bias                    input  value  bias  value
//          \      /                =>       \    /        \    /
//          BiasAdd    value                   Mul          Mul
//                \    /                           \       /
//                  Mul                              AddV2
< ASCII >
// This is to enable the FuseMulAndConv2D pattern.
// Here, root of the result is AddV2 instead of BiasAdd because the value may
// not have rank one and therefore the second operand may not have rank one
// that is required by the BiasAdd. BiasAdd with 'NHWC' data format equivalent
// to AddV2 op.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_27.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/tensorflow/transforms/optimize.td#L91-L98

```c
// This rule does the following pattern match and rewrite:
//
< ASCII >
//       input     bias                    input  value  bias  value
//          \      /                =>       \    /        \    /
//           AddV2    value                   Mul          Mul
//                \    /                           \       /
//                  Mul                             AddV2
< ASCII >
// This is to enable the FuseMulAndConv2D pattern.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_28.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/tf2xla/kernels/image_resize_ops.cc#L41-L80

```c
// We implement bilinear interpolation by upsampling followed by convolution.
// The basic idea is as follows. To scale from NxN to RxR:
//
//    1. S := (N - 1) /  gcd(N-1, R-1)
//    2. k := (R - 1) /  gcd(N-1, R-1)
//    3. Convolution((2k-1)x(2k-1), stride=S, lhs_dilation=k, padding=k-1)
//
// For example, to Scale from 7x7 -> 15x15:
//
//    1. S := (7-1) / gcd(7-1, 15-1) = 6 / gcd(6, 14) = 6 / 2 = 3
//    2. k := (15 - 1) / gcd(7-1, 15-1) = 14 / gcd(6, 14) = 14 / 2 = 7
//    3. Convolution(15x15, stride=3, lhs_dilation=7, padding=2)
//
//
// The 7x7 -> 15x15 case is much too large to write out in full as an
// example. The smallest interesting example is 3x3 -> 4x4.
//
// S := 2
// k := 3
//
< ASCII >
// 00 03 06    00 00 00 00 00 00 00 00 00 00 00      00 02 04 06
// 09 12 15 -> 00 00 00 00 00 00 00 00 00 00 00   -> 06 08 10 12
// 18 21 24    00 00 00 00 00 03 00 00 06 00 00      12 14 16 18
//             00 00 00 00 00 00 00 00 00 00 00      18 20 22 24
//             00 00 00 00 00 00 00 00 00 00 00
//             00 00 09 00 00 12 00 00 15 00 00
//             00 00 00 00 00 00 00 00 00 00 00
//             00 00 00 00 00 00 00 00 00 00 00
//             00 00 18 00 00 21 00 00 24 00 00
//             00 00 00 00 00 00 00 00 00 00 00
//             00 00 00 00 00 00 00 00 00 00 00
//
// with the following convolutional kernel, with stride [2, 2]:
//       1 2 3 2 1
//       2 4 6 4 2
// 1/9 * 3 6 9 6 3
//       2 4 6 4 2
//       1 2 3 2 1
< ASCII >
// Note that the convolution kernel matrix is separable and thus we can instead
// use 2 consecutive 1D kernel of the dimension 2k-1, along each axis.
```
## Visual type:
- #matrix

== ./tensorflow/tensorflow_29.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/tf2xla/kernels/image_resize_ops.cc#L138-L148

```c
// Form a 2D convolution kernel like:
< ASCII >
//       1 2 3 2 1
//       2 4 6 4 2
// 1/9 * 3 6 9 6 3
//       2 4 6 4 2
//       1 2 3 2 1
< ASCII >
// by multiplying two 1D kernels of the form:
// 1/3 * [1 2 3 2 1]
// If the 2D kernel would be very large, the 1D kernel can be applied once in
// each dimension due to the symmetry of the kernel along all axis to reduce the
// computational intensity.
```
## Visual type:
- #matrix

== ./tensorflow/tensorflow_3.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/deadness_analysis.cc#L33-L112

```c
// ALGORITHM OVERVIEW
// ==================
//
// We map every output produced by each node in the TensorFlow graph (including
// control dependence) into an instance of the Predicate class.  Instances of
// Predicate denote logical formulas and mapping a node `n` to a predicate
// `pred` implies that `n` is live whenever `pred` is true.  Then we can deduce
// mismatching liveness in the inputs to node by comparing the predicate those
// inputs are mapped to.  The core logic of this pass resides in creating the
// map from TensorFlow nodes to predicates.
//
//
// MAPPING NODES TO PREDICATES, MODULO CYCLES
// ------------------------------------------
//
// If we ignore cycles for a moment, computing predicates is fairly
// straightforward.  We traverse the graph in a topological order, mapping each
// node to a predicate based on the predicates its inputs are mapped to.  For
// instance a Merge(X, Y) node will be mapped to OR(PredicateFor(X),
// PredicateFor(Y)).  Roughtly speaking, we abstractly interpret each node on
// the "liveness" domain, where values in the domain represent if a tensor
// carries a dead signal or not.
//
//
// DEALING WITH CYCLES
// -------------------
//
// We map Merge nodes that are the target of a backedge to AndRecurrence
// instances.  An AndRecurrence with start() = S and step() = X, printed as
// {S,&,X}, *roughly* represents the infinite list of predicates
// [S,S&X,S&X&X,S&X&X, ...].  So {S,&,X} can be used to represent the predicate
// for Merge in a graph like:
//
< ASCII >
//     Init
//       |
//       v
//     Merge <-----------+
//       |               |
//       v               |
//      Incr             |
//       |               |
//       v               |
//      Switch <- Cond   |
//       |               |
//       v (oidx: 1)     |
//       |               |
//       +---------------+
< ASCII >
//
// Where S is the predicate for Init and X is the predicate that asserts that
// Cond is true.  {S,&,X} states that Merge is live on the first "iteration" iff
// S is true, live on the second iteration iff "S&X" is true, live on the third
// iteration iff "S&X&X" is true etc.  There is a subtlety here, S&X&X would
// normally be equivalent to S&X which isn't quite what we want to represent.
// Instead we want {S,&,X} to denote the infinite list [S, S&X,
// S&X&X',S&X&X'&X'', ...] where X, X', X'' are predicates that assert Cond is
// true on iteration 0, 1, 2 respectively.  This is made more precise in the
// comment on the AndRecurrence class.
//
// The general algorithm that deals with cycles does two topological-order
// iterations over the graph.  On the first iteration it assigns a symbolic
// predicate to merge nodes with backedges.  On the second iteration it tries
// to pattern match the predicates for the backedges of these merges and infer
// an AndRecurrence for the merge.  In other words, we do a data flow analysis
// where the data-flow lattice has two elements, Symbolic and NonSymbolic with
// Symbolic > NonSymbolic.  The lattice has height = 2 so two iterations are
// sufficient to converge.
//
// We first do an optimistic analysis and, if it does not converge, we then fall
// back to a pessimistic analysis.  The optimistic analysis assigns the same
// symbolic predicate to all the merge nodes whose preceding enter nodes have
// the same frame name on the first iteration.  On the second iteration, if all
// the merge nodes are pattern matched into the same AndRecurrence predicate
// instance, the optimistic assignment of the same symbolic predicate is correct
// and the analyzed result is taken.
//
// Otherwise, if the optimistic analysis fails to converge, we then obtain the
// result by falling back to the pessimistic analysis which assigns a unique
// symbolic predicate to each merge on the first iteration.  We still use
// symbolic predicates for merges for which we can't pattern match on the
// backedge predicate.  This is conservatively correct.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_30.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/tf2xla/kernels/tensor_list_utils.cc#L30-L107

```c
// TensorList is represented by a tuple.
// - The first part of the tuple is a buffer containing all the tensors,
// - The following parts are push indices for all nested levels of
//   TensorLists. The last part is push index for the outermost TensorList.
//
// TensorList, as it name suggests, is conceptually a list of tensors. In actual
// representation of a non-nested TensorList, the buffer shape is
// [element_shape, tensor_list_size]. We will call tensor_list_size "leading
// dimension" below. Notice that the leading dimension must be a compile time
// constant, since it's part of the buffer shape.
//
// Example: consider a 3-level nested TensorList whose element type is scalar.
// Assume inner TensorList has leading dimension 4, middle TensorList has 3,
// and outer TensorList has 3.
// Assume that lower cased letter means there is data in that position, and "."
// means there is no data in that position.
< ASCII >
// First element of outer TensorList:
// [ a . . . ]
// [ b c . . ]
// [ d e f . ]
// Second element of outer TensorList:
// [ g h i . ]
// [ j k . . ]
// [ . . . . ]
< ASCII >
// Third element: not pushed yet.
//
// The first part of the tuple is an array of shape [3, 3, 4] containing data.
// The second part is an array of shape [3, 3], each element is push index
// for the inner TensorList. In this case, its values are:
< ASCII >
// [ 1 2 3 ]
// [ 3 2 . ]
// [ . . . ]
< ASCII >
// The third part is an array of shape [3], each element is push index for
// the middle TensorList. In this case, its values are:
< ASCII >
// [ 3 ]
// [ 2 ]
// [ . ]
< ASCII >
// The forth (and last) part is a scalar. It's the push index for the outer
// TensorList. In this case, its values is 2.
//
< ASCII >
// Now imagine we need to push the following element to the outer TensorList:
// [ l . . . ]
// [ m n . . ]
// [ . . . . ]
< ASCII >
// This element is represented by a tuple of 3 parts:
// First part is all data.
// Second part is push indices for the inner TensorList, which is [ 1 2 . ].
// Third part is push index for the middle TensorList, which is 2.
// Now let's do the push.
// First, we append its data to outer TensorList's data.
// Then we start to deal with push indices. Similar to data, we append push
// indices for each level of TensorList.
< ASCII >
// For the inner TensorList: append push indices for the pushed element.
// [ 1 2 3 ]               [ 1 2 3 ]
// [ 3 2 . ] +           = [ 3 2 . ]
// [ . . . ]   [ 1 2 . ]   [ 1 2 . ]
// For the middle TensorList: append push indices for the pushed element.
// [ 3 ]           [ 3 ]
// [ 2 ] +       = [ 2 ]
// [ . ]   [ 2 ]   [ 2 ]
// For the outer TensorList: just add 1.
// 2 + 1 = 3
< ASCII >
//
// Popping an element from the outer TensorList also follows a similar process.
// First part is data. We get data by slicing data with push index for outer
// TensorList (which is 3).
// Second part is push indices for inner TensorList. We get it by slicing
// push indices for inner TensorList with push index for outer TensorList (which
// is 3).
< ASCII >
// [ 1 2 3 ]
// [ 3 2 . ]
// [ 1 2 . ] ===> This is what we want
< ASCII >
// Third part is push index for middle TensorList. We get it by slicing
// push indices for middle TensorList with push index for outer TensorList
// (which is 3).
< ASCII >
// [ 3 ]
// [ 2 ]
// [ 2 ] ===> This is what we want
< ASCII >
```
## Visual type:
- #matrix

== ./tensorflow/tensorflow_31.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/algebraic_simplifier.cc#L5551-L5566

```c
// Match on variadic reduce which computes and returns (min, arg_min).
//
< ASCII >
//                   p0   p2    p1    p3
//                  /|\ \/ |\    |\   /|
//                 / | \/\ | \   | \ / |
//                /  | /\ \|  |  |  /\ |
//               Ne  Lt |  \  |  | |  ||
//                 \ /  |  |\ |  | /  ||
//                  Or /  /  Eq  Lt   ||
//                  | /  /    \  /    //
//                  | |  |     And   //
//                  | |  |      |  //
//                  select     select
//                      \     /
//                       tuple
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_32.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/algebraic_simplifier.cc#L5579-L5594

```c
// Match on variadic reduce which computes and returns (max, arg_max).
//
< ASCII >
//                   p0   p2    p1    p3
//                  /|\ \/ |\    |\   /|
//                 / | \/\ | \   | \ / |
//                /  | /\ \|  |  |  /\ |
//               Ne  Gt |  \  |  | |  ||
//                 \ /  |  |\ |  | /  ||
//                  Or /  /  Eq  Lt   ||
//                  | /  /    \  /    //
//                  | |  |     And   //
//                  | |  |      |  //
//                  select     select
//                      \     /
//                       tuple
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_33.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/ar_crs_combiner.h#L28-L72

```c
// When the HLO graph contains a cross-module AllReduce (N separate AllReduce
// ops that share the same channel_id for MPMD partitioning, or 1 AllReduce op
// for SPMD partitioning), followed by some simple linear operations, followed
// by a cross-replica AllReduce (also known as cross-replica sum, or CRS), we
// can combine the CMAR and the CRAR, to use an efficient AllReduce
// implementation that fully utilizes the interconnect bandwidth.
//
// Such sequences appear in spatially partitioned models (either MPMD or SPMD).
// This pass must run right after spatial partitioning, when the code is still
// in a single HLO module.
//
// The steps are:
// 1) Find CMARs followed by simple ops followed by CRARs.
// 2) Group CMARs by channel_id. They must all be rewritten. For SPMD
//    partitioning, there will only be a single CMAR for each channel_id.
// 3) Prove that the CMAR patterns in each core produce the same result.
// 4) Eliminate the CMAR, and if it feeds an addition/subtraction, divide the
//    other operand by the number of spatial partitions.
// 5) Turn the CRAR into an all-core AllReduce.
//
// The pass also handles the case where multiple CMARs lead to the same CRAR,
// and eliminates all CMARs. This graph:
< ASCII >
//
//        Y
//        |
//  X   CMAR_2   Z
//  |      \    /
// CMAR_1     +
//    \     /
//       +
//       |
//     CRAR
//
// gets rewritten to:
//
//           Z   num_partitions
//            \  /
//       Y    div
//        \   /
//    X     +
//     \   /
//       +
//       |
//  all-core AR
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_34.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/conditional_simplifier.cc#L182-L204

```c
// Removes all unused elements from result tuple. Returns true if anything is
// changed.
//
// Computes and only keeps a subset of result tuple indices which are actually
// being used. This simplification frees up some data-dependencies in branches'
// sub-computations and enables further optimizations.
//
< ASCII >
// *) It is considered the whole tuple is used, and there will be no removal for
//    this case:
//
//        kTuple-result
//              |
//              |
//           kWhile
//
// *) Only index=0 is used, so change (f32[10,10], f32[20,20]) to (f32[10,10])
//    and drop f32[20,20].
//
//        kTuple-result (f32[10,10], f32[20,20])
//              |
//              |
//        get-tuple-element, index=0
< ASCII >
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_35.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/conditional_simplifier.cc#L314-L356

```c
// Merges duplicate(identical) elements in result tuple.
//
// Two tuple elements(indices) are duplicate if they return identical value
// (from the same HloInstruction source) in every branch. In other words, if
// replacing j-th with i-th tuple index results in an invariant, i-th/j-th are
// identical and we can safely replace all GTE j-th (users this conditional
// instruction) with GTE i-th.
//
// Afterwards, any unused j-th tuple index will be removed by
// RemoveUnusedTupleElements and the size of tuple shape will be reduced.
// E.g.
//
< ASCII >
// Before:
//       gte          add
//      /   \        /   \
//      |   |        |   |
//     on_true      on_false
//    (f32, f32)   (f32, f32)
//         |           |
//          \         /
//          conditional
//          (f32, f32)
//            |    |
//           gte  gte
//            \    /
//            tuple
//          (f32, f32)
//
// After:
//       gte          add
//        |            |
//     on_true      on_false
//      (f32)        (f32)
//         |           |
//          \         /
//          conditional
//             (f32)
//               |
//              gte
//              |  \
//              |   |
//              tuple
//            (f32, f32)
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_36.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/conditional_simplifier_test.cc#L412-L434

```c
< ASCII >
// Before:
//       gte          rng
//      /   \        /   \
//      |   |        |   |
//     on_true      on_false
//    (f32, f32)   (f32, f32)
//         |           |
//          \         /
//          conditional
//          (f32, f32)
//
// After:
//       gte          rng
//        |            |
//     on_true      on_false
//      (f32)        (f32)
//         |           |
//          \         /
//          conditional
//             (f32)
< ASCII >
//
// The 'rng' in on_false is to add side-effect so that conditional is not being
// simplified and replaced with 'select' instruction by TryRemoveConditional.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_37.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/convolution_group_converter.cc#L129-L165

```c
// Create a mask for grouped convolution that will make a normal convolution
// produce the same results as a grouped convolution. For a [2, 1, 6]
// filter this returns a [2, 3, 6] mask
< ASCII >
//   1 1 0 0 0 0
//   0 0 1 1 0 0
//   0 0 0 0 1 1
//
//   1 1 0 0 0 0
//   0 0 1 1 0 0
//   0 0 0 0 1 1
//
// The first step is to create a rank 1 constant:
//   0 1 2
//
// This is broadcasted to
//   0 0 0 0 0 0
//   1 1 1 1 1 1
//   2 2 2 2 2 2
//
//   0 0 0 0 0 0
//   1 1 1 1 1 1
//   2 2 2 2 2 2
//
// Then we create another rank 1 constant
//   0 0 1 1 2 2
//
// This is broadcasted to
//   0 0 1 1 2 2
//   0 0 1 1 2 2
//   0 0 1 1 2 2
//
//   0 0 1 1 2 2
//   0 0 1 1 2 2
//   0 0 1 1 2 2
< ASCII >
//
// Finally we use the Eq op of these two broadcasted constants and get the
// desired mask.
```
## Visual type:
- #matrix

== ./tensorflow/tensorflow_38.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion.cc#L101-L138

```c
// Deep copy the given instructions 'from' and 'to' at the ShapeIndexes given in
// 'indices_to_copy'. Add control edges from the respective kCopy instructions
// in deep copy of 'from' to the respective kCopy instruction in the deep copy
// of 'to'.
//
// Requirements: 'from' and 'to' must have compatible shapes.
//
// For example, suppose 'from' and 'to' are two-element tuples where index 0 is
// the only index to copy. Prior to deep-copying we have:
//
//
< ASCII >
//      'from'
//         |
//        ...
//         |
//       'to'
//
// DeepCopyAndAddControlEdges produces:
//
//       'from'
//        /   \
//      GTE   GTE
//       |     |
//     Copy    |
//    /   \   /
//   |    Tuple
//   |      |
//  ctrl   ...
//  edge    |
//   |      |
//   |    'to'
//   |    /   \
//   |  GTE   GTE
//    \  |     |
//     Copy    |
//        \   /
//        Tuple
< ASCII >
//
```
## Visual type:
- #matrix


== ./tensorflow/tensorflow_39.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion.cc#L229-L289

```c
// Add kCopy instructions around the given kWhile instruction to eliminate any
// possible live range interference of HLO values assuming a dependency-based
// ordering. Copies are added conservatively. There  likely are copies which are
// not strictly necessary, but they are removed later in the pass via
// RemoveUnnecessaryCopies.
//
// Elements (each ShapeIndex) in the loop state are considered independently.  A
// copy is added to each element of the loop state which is modified in the
// while body. For each such element, a total of three kCopy instructions are
// added at following locations:
//
< ASCII >
//   (1) The init value is copied before the kWhile instruction. Before:
//
//           (Init)
//             |
//           kWhile
//             |
//            ...
//
//       After:
//
//           (Init)
//             |
//           kCopy
//             |
//           kWhile
//             |
//            ...
//
//       This copy is necessary in case the init value is simultaneously live
//       with the kWhile.
//
//   (2) Copies are added to the parameter and root of the while body
//       computation. Before:
//
//           kParameter
//               |
//              ...
//               |
//           (body root)
//
//       After:
//
//           kParameter
//               |
//             kCopy ----------+
//               |             |
//              ...           ctrl
//               |            edge
//           (body root)       |
//               |             |
//             kCopy <---------+
< ASCII >
//
//       The root kCopy becomes the new root of the computation. Both copies are
//       necessary to any potential interference between the parameter value and
//       the root value. The control edge prevents potential interference
//       between the copies themselves.
//
// If the loop state is a tuple then the above kCopy instructions are a deep
// copy constructed of kCopy, kGetTupleElement, and kTuple instruction as
// constructed by HloInstruction::DeepCopyInstruction.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_4.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/deadness_analysis.h#L23-L43

```c
// This analyzes a TensorFlow graph to identify nodes which may have partially
// dead inputs (i.e. these nodes may have some dead inputs and some alive
// inputs).
//
< ASCII >
// For example, the ADD node in the following graph
//
//      V0  PRED0    V1  PRED1
//       |    |       |    |
//       v    v       v    v
//       SWITCH       SWITCH
//          |            |
//          +---+   + ---+
//              |   |
//              v   v
//               ADD
< ASCII >
//
// can have its inputs independently dead or alive based on the runtime values
// of PRED0 and PRED1.
//
// It is tempting to call this a liveness analysis but I avoided that because
// "liveness" already has other connotations.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_40.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L722-L729

```c
< ASCII >
// Tests Copy Insertion when a while feeds another while
//                         PARAMETER
//                        |        |
//                        GTE(0)   GTE(1)
//                        |        |
//                        X = CreateTuple(GTE(0), GTE(1))
//                                 |
//                        WHILE(X) (root)
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_41.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L792-L796

```c
< ASCII >
// Tests Copy Insertion when a while feeds another while
//                         PARAMETER
//                        |        |
//                         \      /
//                           WHILE(PARAMETER) (root)
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_42.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L851-L855

```c
< ASCII >
// Tests Copy Insertion when a while feeds another while
//                         PARAMETER
//                        |        |
//                         \      /
//                           WHILE(PARAMETER) (root)
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_43.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L959-L972

```c
// Tests while body computation with read-only tuple element 0:
//
< ASCII >
//                         PARAMETER
//                         /       \
//                      GTE(0)     GTE(1)
//                        |  \      |
//                        |   BCAST |
//                        |      \  |
//                        |       ADD
//                        |        |
//                         \      /
//                           TUPLE (root)
< ASCII >
//
// CopyInsertion pass should not generate any copies for the while body.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_44.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L1102-L1130

```c
< ASCII >
// Tests while body computation with nested tuple elements:
//
//                            |
//                    GTE(loop_state, 1)
//                       /          \
// GTE(GTE(loop_state, 1), 0)     GTE(GTE(loop_state, 1), 1)
//           |                              |
//          Add                           Reverse
//           |                              |
//
// CopyInsertion pass will conceptually generate the following, but with the
// actual GTE and Tuple instructions optimized away:
//
//                    Tuple  // old root
//                   /     \
//                  /       \
//                GTE(0)   GTE(1)
//                  |       /  \
//                  |      /    \
//                  |    GTE(0) GTE(1)
//                  |       |    |
//                  |       |   Copy
//                  |       |    |
//                   \      |   /
//                    \    Tuple  // "inner" tuple.
//                     \    /
//                      \  /
//                     Tuple  // new root
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_45.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/copy_insertion_test.cc#L1196-L1218

```c
// Tests while init instruction which has a non-distinct points-to set.
//
//     init = Tuple(Constant(S32, {}), Tuple({vec_one, vec_one}))
//
// CopyInsertion pass will conceptually generate the following, but with some of
// the actual GTE and Tuple instructions optimized away:
//
< ASCII >
//                    Tuple  // old init
//                   /     \
//                  /       \
//                GTE(0)   GTE(1)
//                  |       /  \
//                  |      /    \
//                  |    GTE(0) GTE(1)
//                  |       |    |
//                Copy   Copy   Copy
//                  |       |    |
//                   \      |   /
//                    \    Tuple
//                     \    /
//                      \  /
//                     Tuple  // new init
< ASCII >
//
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_46.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/dynamic_padder.cc#L1881-L1894

```c
// Remove all dynamic shapes between pad-to-static and slice-to-dynamic.
//
< ASCII >
// After this visitor the entry computation then looks like:
//  Param(dynamic)
//    |
//   GTE (dynamic)
//    |
//  PadToStatic(static)
//    |
//   .... regular computation with static shapes.
//    |
//  SliceToDynamic(dynamic)
//    |
// ROOT tuple (dynamic)
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_47.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/flatten_call_graph_test.cc#L194-L203

```c
// Test flattening of a nested calling computations.
//
< ASCII >
//   Entry
//    / \
//    \ /
//     B
//    / \
//    \ /
//     C
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_48.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/hlo_memory_scheduler.cc#L47-L75

```c
// Class implementing a list scheduler of HLO instructions which produces a
// sequence which minimizes memory usage by preferring to schedule the node that
// frees bigger buffer and defines smaller outputs.
//
// Note that list scheduler is a greedy algorithm which cannot guarantee a
// global optimal solution. As a counterexample, considering the following
// graph:
//
< ASCII >
//      +--> B ===> C -------+
// A -> |                    |
//      |                    v
//      +--> D ---> F=======>G
//      |           ^
//      |           |
//      +--> E -----+
//
//  --> : Buffer with size 1
//  ==> : Buffer with size 2
< ASCII >
//
// The list scheduler will always try to defer scheduling B in a greedy way
// since its output buffer is bigger than input. The sequence it creates will
// be:
//   A D E F B C G
// , which has a maximum memory usage of 6 (B is alive while F is executing).
//
// An optimal way to schedule the previous graph is:
//   A B C D E F G
// , which has a maximum memory usage of 5 (when F is executing).
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_49.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/hlo_rematerialization.cc#L194-L205

```c
// Class which maintains an ordered list of instructions with fast insertion
// before arbitrary elements.
//
// This is a skip list structure that has two lanes: express lane and slow lane.
// All nodes are presented on the slow lane but a node can be promoted into
// express lane for fast iteration.
//
< ASCII >
// In the following case, node 2 and node + 1 are connected via an express lane.
//                    +--------------------------+----------->: Express lane
//                    |                          |
//       node1<-> node 2 <-> .. <-> node n <-> node n+1 <->...: Slow lane
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_5.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/deadness_analysis_test.cc#L90-L128

```c
// Creates an induction variable with the following structure (simplified for
// brevity):
//
< ASCII >
//            +---------------+
//            | initial_value |
//            +---------------+
//              |
//              |
//              v
//            +---------------+
//            |     Enter     |
//            +---------------+
//              |
//              |
//              v
//            +---------------+
//         +> |     Merge     | -+
//         |  +---------------+  |
//         |    |                |
//         |    |                |
//         |    v                |
//         |  +---------------+  |
//         |  |  LessThan10   |  |
//         |  +---------------+  |
//         |    |                |
//         |    |                |
//         |    v                |
//         |  +---------------+  |
//    +----+- |    Switch     | <+
//    |    |  +---------------+
//    |    |    |
//    |    |    |
//    |    |    v
//    |    |  +---------------+
//    |    +- |    AddOne     |
//    |       +---------------+
//    |       +---------------+
//    +-----> |     Exit      |
//            +---------------+
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_50.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/reshape_mover_test.cc#L479-L491

```c
// Tree looks like this:
//
< ASCII >
// add1
// |
// +- reshape2 - param2
// |
// +- reshape3 - add0
//               |
//               + reshape0 - param0
//               |
//               + reshape1 - param1
< ASCII >
//
// We expect reshape{0,1} AND reshape{2,3} to be lifted.
```
## Visual type:
- #tree

== ./tensorflow/tensorflow_51.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/cpu/tiled_dot_emitter.cc#L160-L220

```c
// Computes a dot product between "[M,K]{0,1} lhs" with a [K,1] vector (the
// layout of the vector does not matter).  This implementation uses a tiling
// scheme to improve performance.
//
< ASCII >
// We logically separate the LHS matrix into four segments:
//
//   +----------------------+---+
//   |                      |   |
//   |                      |   |
//   |         A            | B |
//   |                      |   |
//   |                      |   |
//   |                      |   |
//   +----------------------+---+
//   |         C            | D |
//   +----------------------+---+
< ASCII >
//
// where A is the largest submatrix of the LHS that can be evenly divided into
// tiles.  For each tile in A, assuming tile_rows_ == tile_cols_ == 4, we have:
//
< ASCII >
//   +---+---+---+---+       +--+--+--+--+
//   |M00|M10|M20|M30|       |V0|V1|V2|V3|
//   +---+---+---+---+       +--+--+--+--+
//   |M01|M11|M21|M31| and   |V0|V1|V2|V3|
//   +---+---+---+---+       +--+--+--+--+
//   |M02|M12|M22|M32|       |V0|V1|V2|V3|
//   +---+---+---+---+       +--+--+--+--+
//   |M03|M13|M23|M33|       |V0|V1|V2|V3|
//   +---+---+---+---+       +--+--+--+--+
< ASCII >
//
// (Legend: rows are horizontal and columns are vertical; and each column is one
// llvm::Value of a vector type)
//
// where:
//
//   a. The left tile is from the column major left matrix.
//   b. The right tile is an elementwise broadcast of a [V0, V1, V2, V3]
//      vector loaded from the RHS vector.
//
// As we iterate through the column dimension, we compute the change to the
// result vector by an elementwise multiplication between the two tiles above
// followed by a reduction along the major dimension:
//
< ASCII >
//                     +-----------------------------------+
//                     | M00*V0 + M10*V1 + M20*V2 + M30*V3 |
//                     +-----------------------------------+
//                     | M01*V0 + M11*V1 + M21*V2 + M31*V3 |
// Result[R:R+4] +=    +-----------------------------------+
//                     | M02*V0 + M12*V1 + M22*V2 + M32*V3 |
//                     +-----------------------------------+
//                     | M03*V0 + M13*V1 + M23*V2 + M33*V3 |
//                     +-----------------------------------+
< ASCII >
//
// Where R is the starting row for the tile.
//
// We have an inner epilogue loop to deal with the "C" submatrix and an outer
// epilogue loop to deal with the B,D submatrix.
//
// TODO(sanjoy): We should investigate if using gather loads and scatter stores
// can be used here have the same inner loop for both column-major and row-major
// matrix-vector products.
```
## Visual type:
- #custom


== ./tensorflow/tensorflow_52.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/cpu/tiled_dot_emitter.cc#L399-L448

```c
// Computes a dot product between "[M,K]{1,0} lhs" with a [K,1] vector (the
// layout of the vector does not matter).  This implementation uses a tiling
// scheme to improve performance.
//
< ASCII >
// We logically separate the LHS matrix into four segments:
//
//   +----------------------+---+
//   |                      |   |
//   |                      |   |
//   |         A            | B |
//   |                      |   |
//   |                      |   |
//   |                      |   |
//   +----------------------+---+
//   |         C            | D |
//   +----------------------+---+
//
// where A is the largest submatrix of the LHS that can be evenly divided into
// tiles.  For each tile in A, assuming tile_rows_ == tile_cols_ == 4, we have:
//
//   +---+---+---+---+
//   |M00|M10|M20|M30|
//   +---+---+---+---+       +--+--+--+--+
//   |M01|M11|M21|M31| and   |V0|V1|V2|V3|
//   +---+---+---+---+       +--+--+--+--+
//   |M02|M12|M22|M32|
//   +---+---+---+---+
//   |M03|M13|M23|M33|
//   +---+---+---+---+
< ASCII >
//
// (Legend: rows are horizontal and columns are vertical; and each row is one
// llvm::Value of a vector type)
//
// where:
//
//   a. The left tile is loaded from the row major left matrix.
//   b. The right vector is loaded from the RHS vector.
//
< ASCII >
// We keep 4 vector accumulators accumulating the following four vector
// expressions as we iterate over the row dimension:
//
//   +------+------+------+------+
//   |M0I*V0|M1I*V1|M2I*V2|M3I*V3|  for I in [0,4)
//   +------+------+------+------+
< ASCII >
//
// In the end we do a horizontal reduction over these 4 vector accumulators to
// get 4 values in the result vector.
//
// We have an inner epilogue loop to deal with the "B" sub-matrix and an outer
// epilogue loop to deal with the C,D submatrix.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_53.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/fusion_merger.h#L27-L60

```c
// An HLO pass that attempts to merge fusion instructions to reduce memory
// bandwidth requirements and kernel launch overhead.
//
// Consider the example below. On the left-hand side, op A is the producer and
// ops B and C are its consumers. FusionMerger duplicates producer ops and fuses
// them into all consumers. The result is depicted on the right-hand side below.
//
< ASCII >
//        p                    p
//        |                  /   \
//        v                 /     \
//        A            +fusion+  +fusion+
//      /   \          |  A'  |  |  A"  |
//     |     |         |  |   |  |  |   |
//     v     v         |  v   |  |  v   |
//     B     C         |  B   |  |  C   |
//                     +------+  +------+
< ASCII >
//
// Op A has been cloned twice and fused with B and C. The kernel launch overhead
// is reduced from 3 to 2. The memory bandwidth requirements may be reduced.
// We trade 1 read of input(A) + 1 write and 2 reads of output(A) for 2 reads of
// input(A). In general the achieveable savings in memory bandwidth depend on
// the differences in memory read and written and the number of consumers. The
// FusionMeger pass takes this into account when making fusion decisions.
//
// The pass traverses the HLO module in reverse post-order (defs before uses).
// Fusion instructions are merged into their users if some conditions are met:
// * The result of merging the fusion instruction into its users would not
//   increase bytes transferred.
// * Producer ops are fusible with _all_ consumers. If they are not fusible with
//   at least one consumers, they won't be fused at all.
// * Producers are kLoop fusion ops.
//
// None of these restrictions are necessary for correctness. In fact, lifting
// the latter two could be beneficial.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_54.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/fusion_merger_test.cc#L46-L55

```c
// Tests that we can merge a fusion instruction that is below threshold.
//
< ASCII >
// Computation after fusion merger pass (Fusion2 is merged into Fusion0 and
// Fusion1):
//                   Param
//                 /   |   \
//          Fusion3 Fusion0 Fusion1
//                 \   |   /
//                   Tuple
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_55.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/gpu_conv_rewriter_test.cc#L405-L418

```c
// Extracted from Inception V3 training.
//
< ASCII >
//                                  filter(HWIO)
//                                  3x3x192x320
//                                      |
//                                      v
//      gradients(NHWC)              reverse
//        20x4x4x320               3x3x192x320
//                    \            /
//                     \          /
//  conv (NHWC) with padding (low=2,high=3,interior=1)
//                     20x10x10x192
< ASCII >
//
// Gradients are padded unevenly.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_56.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/horizontal_loop_fusion.h#L27-L118

```c
// This optimization pass horizontally fuses computations for reducing kernel
// launch overhead while increasing kernel launch dims on GPU. The initial
// motivation of this horizontal fusion is due to the observation that the
// training optimizer phase (e.g., AdamOptimizer and L2Loss, etc.) typically
// has many small kernels as a result of applying the same formula on many
// training parameters (or variables in Tensorflow). Fusing these small
// kernels, hence, provides performance gain.
//
// Theoretically speaking, we may implement a cycle detection algorithm to make
// sure no cycles are created after fusion. However, cycle detection check is
// somewhat cumbersome; also, we observe that naive horizontal fusion of
// arbitrary kernels may not be profitable due to control divergence and
// possible increase of memory bandwidth pressure due to uncoalesced memory
// accesses (note that horizontal fusion does not change the amount of memory
// read+written at all). In practice, a simple yet effective heuristic is used
// to avoid these issues while addressing the known beneficial cases. That is,
// we simply search for fusion candidates by looking for instructions whose
// outputs are all consumed by the same instruction. This catches the cases in
// the training optimizer phase, as the candidate instructions are typically
// consumed only by the ROOT tuple of the entry computation.
//
// The following illustrates the mechanism of the horizontal fusion. Before
// fusion, there are two trivial kernels in the illustrating example. One has
// only a Mul op, while the other consists of only an Add op. Since they are
// only consumed by the same (ROOT) tuple instruction, horizontal fusion is
// triggered.
//
< ASCII >
// i0 i1   i2 i3
//  | |     | |
//  v v     v v
//  Mul     Add
//   |       |
//   v       v
//  (ROOT) tuple
< ASCII >
//
// We fuse into one of two possible patterns, depending on whether all the
// fused operations have the same shape or not.
//
< ASCII >
// case 1: if Mul and Add's output shape and type are the same, then we fuse
// them into the below pattern: i0 i1   i2 i3
//  | |     | |
//  v v     v v
//  Mul     Add
//   |       |
//   v       v
//  (ROOT) tuple
// the fused kernel will be kLoop type, i.e, GPU code is emitted through
// IrEmitterUnnested::EmitLoopFusion
//
// case 2: if Mul and Add's output shape are diffent, then we fuse them into
// the below pattern that adds extra indexing:
// i0 i1   i2 i3       +++ (Slice) Input Fusion
//  | |     | |          +
//  v v     v v          +
//  Mul     Add          +
//   |       |           +
//   v       v           +
// Reshape0  Reshape1    +
//   |       |           +
//   v       v           +
//  Concatenate          +
//   |       |           +
//   v       v           +
//  Slice0  Slice1     +++
//   |       |
//   v       v
// Reshape2  Reshape3
//   |       |
//   v       v
//  (ROOT) tuple
//
// the fused kernel will be kInput type, and, the GPU code is emitted through
// IrEmitterUnnested::EmitInputFusibleNonStridedSlices
< ASCII >
//
// In theory, the pattern in case 1 could also be fused into the case2 target
// graph, but we prefer to fuse into kLoop type, because the codegen for it does
// not have the slicing range check cost introduced by case 2 pattern.
//
// Note that the fusion style by case 2 provides an important advantage that
// kernels of different shapes can be horizontally fused. The first pair of
// reshapes (i.e., Reshape0 and Reshape1) reshape the dims to 1 dimension, so
// that the outputs of the fused kernels can (always) be concatenated. The
// second pair of reshapes (Reshape2 and Reshape3) restore the original shapes
// to the output tensors.
//
// No extra copies are introduced by the horizontal fusion. Besides Reshape2
// and Reshape3, the other instructions are fused into an input fusion; the
// output dims of the concatenate will be used as the kernel launch dims.
// Instruction bitcasts can be used for Reshape2 and Reshape3 as long as the
// outputs of Mul and Add are row-major.
//
// Note, reshapes are added only if the tensors isn't already a vector.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_57.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/multi_output_fusion.h#L36-L92

```c
// Multi-output fusion of sibling and producer-consumer instructions for the
// GPU backend to reduce memory bandwidth requirements.
//
< ASCII >
//   0) Before multi-    1) Sibling multi-    2) Producer-consumer
//      output fusion       output fusion        multi-output fusion
//
//          p                    p                    p
//          |                    |                    |
//          v                    v                    v
//          A                    A               +-fusion--+
//        /   \                  |               |    A    |
//       |     |            +-fusion--+          |   / \   |
//       v     v            |   / \   |          |  B   |  |
//       B     C            |  B   C  |          |  |   |  |
//        \   /             |  |   |  |          |  v   v  |
//         v v              |  v   v  |          |  tuple  |
//        ROOT              |  tuple  |          +---------+
//                          +---------+            /    \
//                            /    \            gte_b  gte_a
//                         gte_b  gte_c           |      |
//                           |      |             |      v
//                            \    /              |      C
//                             v  v                \    /
//                             ROOT                 v  v
//                                                  ROOT
< ASCII >
//
// Multi-output fusion ops have a tuple op at their root containing multiple
// elements as outputs. GetTupleElement ops (depicted as gte_* above) are
// inserted to extract tuple elements for consumers.
//
// The two different flavors of multi-output fusion this pass performs are
// depicted above.
// 1) Fusion of sibling ops reduces memory bandwidth requirements, because
//    common input parameters have to be read only once.
// 2) Fusion of producer-consumer ops reduces memory bandwidth requirements by
//    saving one read from memory. In the example above, B does not need to read
//    the output of A from memory, while C still does (using gte_a).
// Note that sibling (1) and producer-consumer (2) multi-output fusion can be
// combined.
//
// The GpuMultiOutputFusion pass modifies the HLO in reverse post-order (defs
// before uses). First, it attempts to fuse the consumer ops of the current op,
// which are siblings (1). Hereafter, it attempts to fuse the current op with
// one of its consumers (2). This order avoids a phase ordering issue (described
// in go/fusionfusion). It ensures that all GetTupleElement ops inserted as a
// by-product of multi-output fusion will occur before the current op in the
// order of traversal, and hence, not get into the way of subsequent fusion
// attempts.
//
// The GpuMultiOutputFusion pass ensures several conditions are met for fusion.
// Some of them are relevant for correctness. In particular, no cycles must be
// introduced into the HLO module. Moreover, the code emitters for multi-output
// fusion must support the combination of ops and their shapes. Other
// restrictions are rather arbitrary and lifting them could be beneficial.
// * Sibling fusion (1) requires at least one op to be a kFusion.
// * Sibling fusion (1) does not fuse kInput fusions with kLoop fusions, i.e.
//   the fusion kinds must match.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_58.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/gpu/scatter_slice_simplifier.h#L23-L40

```c
// Replaces scatters followed by truncation slices with a new scatter using
// a different output shape, and the slices are eliminated.
//
< ASCII >
// (a) Single output     (b) Multiple outputs    (c) Elementwise users
//
//   T[N+1] scatter       (T1, T2) scatter         T scatter  T constant
//          v                v       v                  v     v
//     T[N] slice          T1 gte    T2 gte            T maximum
//                           v       v                     v
//                         T1 slice  T2 slice           T slice
< ASCII >
//
// This pattern is used when the last element of the scatter output is intended
// to accumulate updates from the input elements that should be ignored.
// This is slow if there are many indices mapped to the last output index and
// the scatter is implemented using atomics, so everything collides on that one
// memory location.
// As OOB scatter indices are dropped by the GPU implementation, we can remove
// the slice step entirely and avoid the memory congestion in the scatter step.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_59.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/spmd/dot_handler.cc#L3973-L3989

```c
// Moves a cluster of memory-reducing nodes into the windowed dot-general loop
// on contracting dimensions. Such a loop has a dynamic slice on the
// non-windowed operand. If we move the input nodes into the loop, the
// dynamic-slice could be merged with them by later optimization passes, which
// reduces memory.
//
< ASCII >
// small_operands             small_operands
//        |                          |
// input_nodes                loop { |
//        |          =>         input_nodes
// loop { |                          |
//    dynamic-slice             dynamic-slice
//    ...                       ...
// }                          }
< ASCII >
//
// Later optimization passes (TpuPadSliceMover) will merge the dynamic slice
// with the input nodes.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_6.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/deadness_analysis_test.cc#L169-L197

```c
// Creates an induction variable with the following structure:
//
< ASCII >
//                           +---------------+
//                           | initial_value |
//                           +---------------+
//                             |
//                             |
//                             v
//                           +---------------+
//                           |     Enter     |
//                           +---------------+
//                             |
//                             |
//                             v
//                           +---------------+
//                           |     Merge     | <+
//                           +---------------+  |
//                             |                |
//                             |                |
//                             v                |
//         +-----------+     +---------------+  |
//         | loop_cond | --> |    Switch     | -+
//         +-----------+     +---------------+
//                             |
//                             |
//                             v
//                           +---------------+
//                           |     Exit      |
//                           +---------------+
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_60.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/service/spmd/dot_handler.cc#L4102-L4123

```c
// Moves a cluster of memory-reducing nodes (with reduce nodes at the end)
// into the windowed dot-general loop on non-contracting dimensions. Such a
// loop has a dynamic-update-slice at the output. If we move the user nodes
// into the loop and before the dynamic-update-slice, the user nodes can
// operate on smaller shapes, which reduces memory.
//
< ASCII >
// small_operands                   small_operands
//  | |                 =>                  | |
//  | |  loop {                     loop {  | |
//  | |    conv                             | broadcast      conv
//  | |      |                              |     |           /
//  | | dynamic-update-slice                |  dynamic-slice /
//  | |         |                           |     |         /
//  | |  }      |                           |  multiply-----
//  |broadcast  /                           |    /
//  | |        /                            reduce
//  |multiply--                             |
//  \ |                                dynamic-update-slice
//   reduce                         }
< ASCII >
//
// Later optimization passes (TpuPadSliceMover) will merge the dynamic slice
// with the input nodes (broadcast).
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_61.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/xla/tests/concat_test.cc#L798-L806

```c
// Regression test for b/31944287. x*y is used (at the same index) by all
// operands of the concat. We should emit x*y in three incoming basic blocks of
// the concat because these basic blocks are not control-equivalent.
//
< ASCII >
//      x*y
//    /  |   \
// add1 add2 add3
//    \  |   /
//     concat
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_62.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/common_runtime/isolate_placer_inspection_required_ops_pass.h#L22-L55

```c
// Adds Identities for each input/output of function-calling ops.
//
// For example, the following graph calling a function on inputs `a` and `b`
// and producing output `y` will be rewritted to include identities on all
// edges:
//
< ASCII >
//      a             b
//      |             |
//      v             v
//    f (PartitionedCallOp)
//         |
//         v
//         y
//
// is transformed to
//
//      a             b
//      |             |
//  a_f (Identity)   a_f (Identity)
//      |             |
//      v             v
//    f (PartitionedCallOp)
//         |
//      f_y (Identity)
//         |
//         v
//         y
< ASCII >
//
// This pass is currently needed to simplify correctly placing the nodes
// producing inputs for as well as consuming output from function-calling ops.
//
// This pass should also help to implement replacing PartitionedCallOp with
// component function calls (to avoid copying input/output tensors), if we get
// to it.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_63.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/common_runtime/lower_while_op.cc#L33-L62

```c
// Helper to convert a functional While op to its lowered form.
//
< ASCII >
// Example:
//
// Input graph:
//
// loop_var -> WhileOp<cond_func, body_func> -> consumer
//
// Output graph(top to down flow):
//
//                   loop_var
//                      |
//                    Enter
//                      |
//  cond_func ---<--- Merge  ---<--- NextIteration
//      |               |                |
//      V               V                ^
//      |               |                |
//  LoopCond  --->--- Switch --->--- body_func
//                      |
//                     Exit
//                      |
//                   consumer
//
// DT_RESOURCE tensors are handled specially:
//
// resource_loop_var -> Enter[is_constant=True] -> cond_func and body_func
//      |
//      V
//   consumer
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_64.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/common_runtime/placer_inspection_required_ops_utils.h#L123-L150

```c
// Adds Identities for each input and output of function-calling ops in `graph`
//
// For example, the following graph calling a function on inputs `a` and `b`
// and producing output `y` will be rewritten to include identities on all
// edges:
//
< ASCII >
//      a             b
//      |             |
//      v             v
//    f (PartitionedCallOp)
//         |
//         v
//         y
//
// is transformed to
//
//      a             b
//      |             |
//  a_f (Identity)   b_f (Identity)
//      |             |
//      v             v
//    f (PartitionedCallOp)
//         |
//      f_y (Identity)
//         |
//         v
//         y
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_65.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/common_runtime/quantize_training.cc#L243-L247

```c
// Add a restore subgraph for each variable and connect to the restore_all op.
// For each variable we add the following subgraph:
< ASCII >
//           Assign----restore_all
//          |      |
//   RestoreV2    Variable
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_66.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/common_runtime/quantize_training.cc#L407-L417

```c
// Creates an automatically initialized exponential moving average variable.
// This uses a switch op to assign a value to the variable on the first run,
// and update with the moving average for all other runs:
< ASCII >
//                   init_val
//                      |
//      var--is_init--switch
//       |      true /      \ false
//       |          |        |
//       |         EMA    init_val
//       |           \      /
//       +----------- assign
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_67.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/graph/collective_order_test.cc#L97-L109

```c
// Initialize the following graph:
//
< ASCII >
//       (cpu0) (cpu1)
//         a      b
//         |      |
//         c1     c1
//         |      |
//         id     id
//        /  \   /  \
//       c2  c3 c2  c3
< ASCII >
//
// Here ci denotes a collective node with `instance_key` i.  `a` and `b` are
// inputs, `id` is identity node.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_68.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/graph/collective_order_test.cc#L154-L165

```c
// Initialize the following graph:
//
< ASCII >
//         a
//         |
//         c1
//        /  \
//       c4  id
//          /  \
//         c2  c3
< ASCII >
//
// Here ci denotes a collective node with `instance_key` i.  `a` is an input,
// `id` is identity node.
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_69.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/graph/collective_order_test.cc#L196-L201

```c
// Initialize the following graph:
//
< ASCII >
//         w   x   y   z
//         |   |   |   |
//         c1  c2  c3  c4
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_7.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/partially_decluster_pass.cc#L162-L187

```c
// Clones nodes to outside their cluster to avoid device-to-host copies.  For
// instance, converts this:
//
< ASCII >
//         .....
//           |
//           v
//      A_Clustered ====> C_Unclustered
//           |
//           v
//      B_Clustered
//
// to:
//
//         .....
//          | |
//          | +-------------+
//          |               |
//          v               v
//      A_Clustered   A_Unclustered ====> C_Unclustered
//           |
//           v
//      B_Clustered
< ASCII >
//
// where the ===> arrow has a hostmem source and destination and would entail a
// device to host copy if the source and destination were not in the same XLA
// cluster.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_70.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/arithmetic_optimizer.cc#L517-L543

```c
// Rewrite a tree of Add/AddN with a single AddN operation, consuming all the
// original inputs of absorbed nodes.
//
// 1) All nodes must have the same device placement.
//
// 2) If All nodes in a Add/AddN subgraph have symbolically equal shape, tree is
//    optimized to a single AddN node.
< ASCII >
//
//                AddN_1
//             /    |    \
//          Add_1   z   Add_2       -> AddN(x, y, z, w, q, e)
//          /  \        /  \
//         x    y      w    Add_3
//                          / \
//                         q   e
< ASCII >
//
// 3) If some nodes have different shape (it needs to be broadcastable to the
//    shape of a "root), tree is optimized to AddNs for symbolically equal
//    shapes, and a tree of Add ops, that minimize broadcasts.
< ASCII >
//
//                AddN_1                                 Add
//             /    |    \                              /  \
//          Add_1   z   Add_2       ->               Add    w
//          /  \        /  \                        /   \
//         x    y      w    Add_3      AddN(x, y, q, e)  z
//                          / \
//                         q   e
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_71.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/arithmetic_optimizer.cc#L970-L981

```c
// Binary associative ops can be re-ordered to minimize the number of broadcasts
// and the size of a temporary tensors.
//
< ASCII >
// Example: [a, c] - scalars, [b, d] - matrices
//   @ - binary associative op (Add or Mul)
//   @* - broadcast
//
//           @                      @*
//        /     \                /      \
//      @*       @*      ->     @        @
//    /   \    /   \          /   \    /   \
//   a     b  c     d        a     c  b     d
< ASCII >
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_72.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/arithmetic_optimizer.cc#L1976-L1988

```c
// Bypass redundant reshape nodes:
//
< ASCII >
//   Reshape                    Reshape  <-+
//      ^                                  |
//      |                                  |
//   Reshape       becomes      Reshape    |
//      ^                                  |
//      |                                  |
//    input                      input  ---+
< ASCII >
//
// Additionally,  Reshape and BroadcastTo nodes where the
// input and target shapes are equal are bypassed.
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_73.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/arithmetic_optimizer.cc#L2237-L2257

```c
// Fold a multiply of a scalar into the following convolution. This folding
// can jump across nodes that merely reorders data (such as reshape and
// transpose). For example, we can optimize
//
//
< ASCII >
//         Conv2D                             Conv2D
//        /      \                           /      \
//    Transpose  weights*       ->     Transpose    Mul
//       |                                |        /   \
//      Mul                               |    weights  scale
//     /   \                              |
//   input  scale**                     input
< ASCII >
//
//  *) weights must be a const
// **) scale must be a const scalar
//
// When `weights` and `scale` are constant, `Mul` in the optimized graph can be
// constant-folded, also weights tend to be smaller than the activations.
//
// TODO(jingyue): Fold scalar multiplies to Conv?DBackpropFilter and
// Conv?DBackpropInput.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_74.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/implementation_selector.cc#L47-L100

```c
// TODO(b/157615690): clean up function implementation swap code.
// The overall idea for the function swap is like below:
< ASCII >
//          -----------                            -----------
//  inp_1 ->|  P_C    | -> out_1         g_inp_1 ->|  P_C    | -> g_out_1
//  inp_2 ->| forward | -> out_2         g_inp_2 ->| backward| -> g_out_2
//          | FUNC_1  | -> out_3         g_inp_3 ->| FUNC_1  |
//          -----------                            -----------
//           |  |  |                                 ^  ^  ^
//           v  v  v                                 |  |  |
//           s1 s2 s3                                s1 s2 s3
//           |                                       ^
//           |                                       |
//           |             --------------            |
//           |-----------> | Identity_1 | ---------->|
//                         --------------
< ASCII >
// P_C: op Partitioned_call or stateful_partitioned_call
// FUNC1 (forward): TF function generated for the forward path.
// FUNC1 (backward): TF function generated for the backward path.
// inp_x: input tensors for the forward path.
// out_x: output tensors for the forward path.
// g_inp_x: gradient input tensors for the backward path.
// g_out_x: gradient output tensors for the backward path.
// s_x: intermediate result generated by forward tf function, which will be
//      consumed by backward function for gradient calculation.
//
// In the example above, the FUNC_1 takes 2 inputs, and return 3 outputs, in the
// meantime, generate 3 intermediate results for gradient calculation.
// The backward function will take 6 inputs, 3 for the gradient value for out_x,
// and 3 for the intermediate results s1/2/3. It returns 2 outputs for gradient
// value wrt inp_x.
//
// Given the graph, especially after the device placement is done, we could
// check if there is an alternative FUNC_2 that is better for the assigned
// device type. Note that FUNC_2 (both forward and backward) should have same
// amount of input output tensor with same dtype. However, it can generate
// different intermediate state tensor, both number wise and type wise, since it
// depends on the implementation detail.
//
// Also note that there might be some Identity op being added to the output of
// the forward function by IsolatePlacerInspectionRequiredOps for device
// placement. When the output DTYPE changes when switching from FUNC_1 to
// FUNC_2, the Identity node down the stream also need to be updated with new
// DTYPE.
//
// Based on this, the rewrite need to happen for following items:
//
// 1. P_C forward/backward need to use FUNC_2 instead of FUNC_1.
// 2. The T_IN for P_C backward need to be updated since the s_x can be
//    different between FUNC_1 and FUNC_2.
// 3. The T_OUT for P_C forward need to be updated since the s_x can be
//    different between FUNC_1 and FUNC_2.
// 4. The input edge for P_C backward need to be updated since the amount of
//    intermediate result can be different between FUNC_1 and FUNC_2.
// 5. DTYPE of the Identity node after s_1/2/3 need to be updated if they exist.
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_75.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/optimizers/scoped_allocator_optimizer.cc#L189-L214

```c
// In certain cases, we would like to insert an identity op between `input` and
// `op` to ensure correctness.  We currently do this in 2 cases: when `input` is
// Exit node, or when `input` is already marked for allocation with another
// scoped allocator op.
//
// If `input` is an Exit node, we add an identity to avoid the case when Exit
// has inputs from different frames.
//
// If `input` is in `sa_opti->repeated_outputs()`, this means that it will be
// potentially used by multiple scope ids.  Since there can be only one scope id
// per output, we insert an identity between the input and op.  This will ensure
// that the identity becomes the new input to op, and this identity can be
// marked with a new scope id different from `input`.
//
// If the graph is rewritten, this function will perform the following change:
//
< ASCII >
//  input                                  input
//   |                                      |
//   op                                  Identity
//                                          |
//                                          op
< ASCII >
//
// This function returns the input to op in `new_input`, and the output index
// from input to op in `new_output_index`.
// `edge_name` gives the name of the edge from `input` to `op`, and
// `output_index` is the output index of this edge on `input`.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_76.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/grappler/utils/pattern_utils.h#L25-L117

```c
//------------------------------------------------------------------------------
// A pattern can be defined by the following grammar. Here, op_type is any valid
// op name in the TensorFlow.
//
//    leaf_pattern ::= `{` op_type `}`
//    pattern ::= leaf_pattern |
//                `{` op_type `,` `{` pattern `,` ... `,` pattern `}` `}`
//
// (1) For example, the following pattern syntax describes a pattern for
// _FusedConv2D (Conv2D + BiasAdd + Relu). Note that "*" means any type of op.
//
//  {"Relu",
//    {
//      "BiasAdd",
//      {
//        {"Conv2D"},
//        {"*"}
//      }
//    }
//  }
//
// The syntax above has a root ("Relu") and children (inputs), where each child
// is a sub-pattern. Graph pattern matcher finds a match for the given pattern
// syntax in a graph and returns a set of matched nodes.
//
// (2) In order to match a DAG with a given root, we extend pattern syntax with
// labels. For example, a frequently found pattern in Deep Learning models is a
// residual block like below.
//
< ASCII >
//    Placeholder  Const
//          |        |
//    +-----+-----+  |
//    |           |  |
//    |           v  v
//    |          Conv2D   Const
//    |            |        |
//    |            v  v-----+
//    |          BiasAdd
//    |            |
//    v v----------+
//   AddV2
< ASCII >
//
// As shown above, it is the same input node (Placeholder) consumed by both
// AddV2 and and Conv2D. This constrained can be put as labels in the following
// augmented pattern syntax.
//
//  {"AddV2", "my_add",
//    {
//      {"*", "my_residual_input"},
//      {"BiasAdd", "my_bias_add",
//        {
//          {"Conv2D", "my_conv",
//            {
//              {"*", "my_residual_input"},
//              {"*", "my_filter"}
//            }
//          },
//          {"*", my_bias"}
//        }
//      }
//    }
//  }
//
// Note that the same label "my_residual_input" is used to tell that it is a
// child of both "AddV2" and "Conv2D". Labels are arbitrary strings to associate
// with the nodes to be matched as well as to uniquely identify those nodes.
//
// (3) The motivatation for a grammar based pattern matching in grappler is to
// make easy for finding fusion pattern in the remapper. A subgraph that
// matches a given pattern, however, is not fusable if any of the matched node,
// that will be removed as a part of fusion, has a consumer outside the matched
// subgraph. In order to check for such type of external dependencies, we
// further extend pattern syntax by prospective action (NodeStatus) on the
// matched nodes as shown below. This helps cross checking the nodes to be
// removed with the nodes matched intially.
//
//  {"AddV2", "my_add", NodeStatus::kReplace,
//    {
//      {"*", "my_residual_input", NodeStatus::kRemain},
//      {"BiasAdd", "my_bias_add", NodeStatus::kRemove,
//        {
//          {"Conv2D", "my_conv", NodeStatus::kRemove,
//            {
//              {"*", "my_residual_input", NodeStatus::kRemain},
//              {"*", "my_filter", NodeStatus::Remain}
//            }
//          },
//          {"*", my_bias", NodeStatus::kRemain}
//        }
//      }
//    }
//  }
//------------------------------------------------------------------------------
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_77.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/lib/strings/ordered_code.cc#L317-L376

```c
// Signed number encoding/decoding /////////////////////////////////////
//
// The format is as follows:
//
// The first bit (the most significant bit of the first byte)
// represents the sign, 0 if the number is negative and
// 1 if the number is >= 0.
//
// Any unbroken sequence of successive bits with the same value as the sign
// bit, up to 9 (the 8th and 9th are the most significant bits of the next
// byte), are size bits that count the number of bytes after the first byte.
// That is, the total length is between 1 and 10 bytes.
//
// The value occupies the bits after the sign bit and the "size bits"
// till the end of the string, in network byte order.  If the number
// is negative, the bits are in 2-complement.
//
//
< ASCII >
// Example 1: number 0x424242 -> 4 byte big-endian hex string 0xf0424242:
//
// +---------------+---------------+---------------+---------------+
//  1 1 1 1 0 0 0 0 0 1 0 0 0 0 1 0 0 1 0 0 0 1 0 0 0 1 0 0 0 0 1 0
// +---------------+---------------+---------------+---------------+
//  ^ ^ ^ ^   ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^
//  | | | |   | | | | | | | | | | | | | | | | | | | | | | | | | | |
//  | | | |   payload: the remaining bits after the sign and size bits
//  | | | |            and the delimiter bit, the value is 0x424242
//  | | | |
//  | size bits: 3 successive bits with the same value as the sign bit
//  |            (followed by a delimiter bit with the opposite value)
//  |            mean that there are 3 bytes after the first byte, 4 total
//  |
//  sign bit: 1 means that the number is non-negative
//
// Example 2: negative number -0x800 -> 2 byte big-endian hex string 0x3800:
//
// +---------------+---------------+
//  0 0 1 1 1 0 0 0 0 0 0 0 0 0 0 0
// +---------------+---------------+
//  ^ ^   ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^
//  | |   | | | | | | | | | | | | | | | | | | | | | | | | | | |
//  | |   payload: the remaining bits after the sign and size bits and the
//  | |            delimiter bit, 2-complement because of the negative sign,
//  | |            value is ~0x7ff, represents the value -0x800
//  | |
//  | size bits: 1 bit with the same value as the sign bit
//  |            (followed by a delimiter bit with the opposite value)
//  |            means that there is 1 byte after the first byte, 2 total
//  |
//  sign bit: 0 means that the number is negative
< ASCII >
//
//
// Compared with the simpler unsigned format used for uint64 numbers,
// this format is more compact for small numbers, namely one byte encodes
// numbers in the range [-64,64), two bytes cover the range [-2^13,2^13), etc.
// In general, n bytes encode numbers in the range [-2^(n*7-1),2^(n*7-1)).
// (The cross-over point for compactness of representation is 8 bytes,
// where this format only covers the range [-2^55,2^55),
// whereas an encoding with sign bit and length in the first byte and
// payload in all following bytes would cover [-2^56,2^56).)
```
## Visual type:
- #memory-layout

== ./tensorflow/tensorflow_78.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/tfrt/utils/graph_partition.h#L28-L55

```c
// Inserts send/recv ops to `graph` if nodes are assigned to multiple devices.
// Specifically, nodes on the same device will be wrapped in a function and
// invoked by a PartitionedCall op. All PartitionedCall ops are connected to a
// StatefulPartitionedCall op (which behaves as a 'stateful IdentityN') to
// protect them from being pruned in the subsequent MLIR lowering passes
// (b/232026253).
//
// The following shows a simple example of using this method.
//
< ASCII >
// The original graph has four nodes that are placed on different devices.
//
//        ----->  op1(host)  ------
//       /                         \
//   input(host)               output(host)
//       \                         /
//        -----> op2(device) ------
< ASCII >
//
// Calling this method will return the following graph, where `op1` is wrapped
// in the function invoked by `PartitionedCall_1`, and `op2` is wrapped in the
// function invoked by `PartitionedCall_2`. Both of them have a data dependency
// with the `StatefulPartitionedCall` op.
//
< ASCII >
//   input ---> PartitionedCall_1 ----
//                                    \
//                         StatefulPartitionedCall ---> output
//                                    /
//              PartitionedCall_2 ----
< ASCII >
//
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_79.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/transforms/constant_folding/pass.cc#L2465-L2484

```c
// Consider the transformation
//
< ASCII >
//                      +                +       = parent
//                     / \              / \
//                    C   +    -- >    X   +     = children
//                       / \              / \
//                      X   Y            C   Y   = leaves
< ASCII >
//
// where C is constant, X is non-constant, Y may be constant or non-constant,
// and '+' denotes an associative and commutative operator like addition or
// multiplication. This optimization pushes constants down in the tree to
// canonicalize it. Moreover, in cases where the child node has a second
// constant input Y we will create a leaf node that can be folded, e.g.
//
//    Add(C1, Add(C2, X)) -> Add(X, Add(C1, C2)) -> Add(X, C1 + C2)
//
// We also handle the non-commutative cases of subtraction and division
// by rotating the tree locally, e.g.
//    Sub(C, Add(X, Y)) -> Sub(Sub(C, Y), X)
//    Mul(C, Div(X, Y)) -> Mul(X, Div(C, Y)).
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_8.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/jit/resource_operation_safety_analysis.cc#L16-L79

```c
// ALGORITHM OVERVIEW
// ==================
//
// An XLA cluster hoists all resource reads to be beginning of the cluster
// execution and all the resource writes to the end.  This means it cannot
// enforce arbitrary ordering dependencies (via control or data edges) between
// resource operations.  Since all resource reads happen before all resource
// writes, edges constraining resource reads to happen before resource writes
// are fine, but all other kinds of edges are problematic.  This analysis
// computes the set of pairs of resource operations that cannot be put in the
// same cluster because XLA cannot respect the dependencies between them in the
// TensorFlow program.
//
// TODO(b/112856632): We can, in theory, support Read->Read and Write->Write
// dependencies.
//
// Specifically the result computed by this analysis contains the edge {W, R}
// iff all of these hold true:
//
//   - In the graph (g - {edges from NextIteration to Merge}) there is a path
//     from W to R.
//   - IsEdgeSafe(W, R) == False [defined below]
//   - W != R (note: some resource operations both read from and write to
//     resource variables).
//
// The result is incorrect around loops because we ignore edges from
// NextIteration to Merge.  For instance, in:
< ASCII >
//
// Init -----> Merge <-------+
//               |           |
//               v           |
//             Read          |
//               |           |
//               v           |
//             Write         |
//               |           |
//               v           |
//           NextIteration --+
< ASCII >
//
// we won't put (Read, Write) in the returned set.  This is fine if
// auto-clustering can only cluster the Read->Write edge, but it is a problem if
// it clusters the Write->NextIteration->Merge->Read edges instead.  So we rely
// on auto-clustering to not cluster NextIteration->Merge edges.  The same
// problem is present for the functional version of the loop above and we also
// rely on auto-clustering not clustering functional while loops containing
// resource operations.
//
// One way to think about this is that we only care about cases where two nodes,
// A and B, would normally have been put in the same cluster but cannot legally
// be in the same cluster because of resourcevar-dependencies.  If A and B would
// normally have been put in the same cluster then all paths between A and B
// would have to be clusterable (otherwise we'd have introduced a cycle).  Ergo
// there could not have been a NextIteration->Merge edge between A and B since
// we don't cluster these edges.
//
// IMPLEMENTATION
// --------------
//
// We traverse the graph minus backedges in reverse post order, mapping each
// node to the set of resource operation reaching that node.  Since we visit
// producers before consumers, we can construct the set of reaching operations
// by taking the union of the operations reaching the input nodes.  These
// "reaching resource operations" can then be used to create the pairs of
// incompatible nodes using `IsEdgeSafe`.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_80.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/transforms/constant_folding/pass.cc#L3175-L3205

```c
// This implements constant push-down for BiasAdd. In the following "CV" is a
// constant vector (tensor of rank 1), "V" is a (possibly) non-constant vector,
// "CM" is a matrix (tensor of rank >= 2), "M" is a (possibly)
// non-constant matrix, and "BA" is BiasAdd.
// For a valid input graph, the following 4 rewrites are legal:
//
< ASCII >
//  1)                  +                +
//                     / \              / \
//                    BA  CV    -- >   BA  V
//                   / \              / \
//                  M   V            M   CV
//
//  2)                  +                +
//                     / \              / \
//                    BA  CM    -- >   BA  M
//                   / \              / \
//                  M   V            CM  V
//
//  3)                  BA               BA
//                     / \              / \
//                    +  CV     -- >   +   V
//                   / \              / \
//                  M   V            M  CV
//
//  4)                  BA               BA      = parent
//                     / \              / \
//                    BA  CV    -- >   BA  V     = children
//                   / \              / \
//                  M   V            M  CV       = leaves
< ASCII >
//
// Cases 1 through 3 have additional sub-cases due to the symmetry of Add.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_81.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/core/transforms/constant_folding/pass.cc#L3281-L3311

```c
// This implements constant push-down for Add. In the following "CV" is a
// constant vector (tensor of rank 1), "V" is a (possibly) non-constant vector,
// "CM" is a matrix (tensor of rank >= 2), "M" is a (possibly)
// non-constant matrix, and "BA" is BiasAdd.
// For a valid input graph, the following 4 rewrites are legal:
//
< ASCII >
//  1)                  +                +
//                     / \              / \
//                    BA  CV    -- >   BA  V
//                   / \              / \
//                  M   V            M   CV
//
//  2)                  +                +
//                     / \              / \
//                    BA  CM    -- >   BA  M
//                   / \              / \
//                  M   V            CM  V
//
//  3)                  BA               BA
//                     / \              / \
//                    +  CV     -- >   +   V
//                   / \              / \
//                  M   V            M  CV
//
//  4)                  BA               BA      = parent
//                     / \              / \
//                    BA  CV    -- >   BA  V     = children
//                   / \              / \
//                  M   V            M  CV       = leaves
< ASCII >
//
// Cases 1 through 3 have additional sub-cases due to the symmetry of Add.
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_82.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/graph_info_test.cc#L606-L619

```c
< ASCII >
//                                         ________________
// A more complex case: [tensor], (node), (partitioned node)
//        _           _           _           _
// [0]-->(0)-->[1]-->(1)-->[5]-->(4)-->[6]-->(5)-->[7]
//                     \
//                      \
//                       \>[2]-->(2)-->[3]-->(3)-->[4]
//
// Greedy partitioning;
//         ____
//  [0]-->(0145)-->[7]
//            \
//             \-->[2]-->(23)-->[4]
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_83.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/graph_info_test.cc#L652-L657

```c
// Same, non-greedy partitioning:
< ASCII >
//         __           __
//  [0]-->(01)-->[5]-->(45)-->[7]
//          \
//           \-->[2]-->(23)-->[4]
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_84.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/delegate_test_util.h#L137-L155

```c
// Tests scenarios involving a single delegate and control edges.
< ASCII >
// Subgraph 0 has the form
//
//         /---OP2---\
//        /           \
// >---OP0             OP3--->
//        \           /
//         \---OP1---/
//
// Delegating OP0, OP2 will generate an execution graph with a "super-node"
// {OP0->OP2}, which can be disabled by adding (in metadata) a control edge
// between OP1 and OP2:
//
//         /->-OP2---\
//        /     ^     \
// >---OP0      ^      OP3--->
//        \     ^     /
//         \---OP1---/
< ASCII >
//
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_85.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.cc#L940-L948

```c
< ASCII >
//      input
//        |
//   convolution
//     /     \
//   tanh    cos
//     \     /
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph

== ./tensorflow/tensorflow_86.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.cc#L1084-L1098

```c
< ASCII >
//      input
//        |
//   convolution
//      /    \
//   tanh    cos
//    /     /   \
//   |    prelu  sin
//   |      \   /
//   |       pow
//   |        |
//   |       exp
//    \       |
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_87.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.cc#L1309-L1323

```c
< ASCII >
//                input1
//                  |
//              convolution
//                  |
//         input0  cos0
//             \   /
//              add
//               |
//              cos1
//               |
//              sin
//               |
//              abs
//               |
//             output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_88.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.cc#L1501-L1507

```c
< ASCII >
//     input1
//       |
//     slice
//       |
//      cast
//       |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_89.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.cc#L1599-L1607

```c
< ASCII >
//       input
//         |
//      Reshape
//       /   \
//     Add   Add
//       \   /
//        Mul
//         |
//       output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_9.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/compiler/mlir/lite/experimental/tac/transforms/compute_cost.cc#L44-L70

```c
// We will caculate the total compute cost for each Func Op.
//
// The compute cost is simply an add-up of the costs of all the operations
// within the FuncOp. (Excluding const ops since they're just "data".)
// We will ignore quant/dequant/requant costs within the Func Op as well,
// intuition:
//
// The assumpution is that quant/dequant/requant will only happen at the begin
// and the end of the FuncOp (basically the "boundaries" of the subgraph).
// So we can imagine if multiple "same-inference-typed" graph are presented at
// the same time, the quant/dequant ops pair can be squashed:
//
< ASCII >
//         dequant         ------------
//            |
//          ops...             FuncOp1
//            |
//         quant           -------------
//           |         <--- can be squashed
//         dequant         -------------
//            |
//        ops...               FuncOp2
//           |
//         quant          ---------------
< ASCII >
//
// But it's true quant & dequant ops can happen "within" the FuncOp as well,
// normally as "quantization params" adjust. We should check more careful to
// include those as those ops wouldn't be "squashed".
```
## Visual type:
- #custom

== ./tensorflow/tensorflow_90.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L25-L31

```c
< ASCII >
//    input
//      |
// convolution
//      |
//   cosinus
//      |
//   output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_91.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L34-L42

```c
< ASCII >
//      input0
//        |
//   convolution   input1
//          \        /
//        multiplication0    input2
//                 \         /
//               multiplication1
//                      |
//                    output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_92.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L46-L58

```c
< ASCII >
//      input0(32x32x128)
//        |
//   convolution       input1(32x32x1)
//        |                /
//   conv_out(32x32x16)   /
//          \            /
//          broadcast_mul   input2(32x32x16)
//                |            /
//       mul_out(32x32x16)    /
//                 \         /
//               multiplication
//                      |
//                    output(32x32x16)
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_93.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L62-L74

```c
< ASCII >
//      input0(32x32x128)
//        |
//   convolution       input1(32x32x16)
//        |                /
//   conv_out(32x32x16)   /
//          \            /
//          multiplication   input2(1x1x16)
//                |            /
//       mul_out(32x32x16)    /
//                 \         /
//               broadcast_mul
//                      |
//                    output(32x32x16)
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_94.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L78-L88

```c
< ASCII >
//      input0
//        |
//   convolution   input1
//          \        /
//        multiplication0    input2
//                 \         /
//               multiplication1
//                      |
//                   cosinus
//                      |
//                   output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_95.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L92-L100

```c
< ASCII >
//      input
//        |
//   convolution
//     /     \
//   tanh     |
//     \     /
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_96.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L104-L112

```c
< ASCII >
//      input
//        |
//   convolution
//     /     \
//    |     tanh
//     \     /
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_97.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L116-L124

```c
< ASCII >
//      input
//        |
//   convolution
//     /     \
//   tanh    cos
//     \     /
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_98.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L128-L142

```c
< ASCII >
//      input
//        |
//   convolution
//      /    \
//   tanh    cos
//    /     /   \
//   |    prelu  sin
//   |      \   /
//   |       pow
//   |        |
//   |       exp
//    \       |
//  substraction
//        |
//     output
< ASCII >
```
## Visual type:
- #program-graph


== ./tensorflow/tensorflow_99.md
# https://github.com/tensorflow/tensorflow/blob/5f3b48c8283266df2e17cfc2f1868397405cb5d0/tensorflow/lite/delegates/gpu/common/gpu_model_test_util.h#L145-L159

```c
< ASCII >
//                input1
//                  |
//              convolution
//                  |
//         input0  cos
//             \   /
//              add
//               |
//              cos
//               |
//              sin
//               |
//              abs
//               |
//             output
< ASCII >
```
## Visual type:
- #program-graph
