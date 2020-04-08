# <div align='center'><i>Neural Style Transfer</i></div>


<div align='left'>Neural style transfer is a <i>Deep Learning</i> computer vision technique for transferring the artistic style of the source image to the target image while preserving the content of the target image to create new artworks. This technique was pioneered by <b>Leon A. Gatys</b> in his paper <a href="https://arxiv.org/abs/1508.06576"><i>A Neural Algorithm of Artistic Style</i></a>.<br></div>
<p></p>
<div align='left'>      The key finding of this paper is that the representations of content and style in the Convolutional Neural Network are well separable. That is, we can manipulate both representations independently to produce new, perceptually meaningful images. This is based on the fact that CNN's extract features hierarchically. The initial layers of the CNN might extract simple features such as edges,corners, etc(ie detailed pixel value) while the deeper layers extract more complex features such as shapes, regions of the image, etc. Based on this we can say that the style of the image can be extracted from the initial layers while the content can be extracted from deeper layers. In this context, style essentially means textures, colors, and visual patterns in the image, at various spatial scales; and the content is the higher-level macrostructure of the image.</div>

<p align='center'><img src=./images/test.png></p>
<p></p>
The key notion behind all deep-learning algorithms is to define a loss function to specify what you want to achieve, and you minimize this loss. Here in <i>Style Transfer</i>, our goal is to conserve the content of the original image while adopting the style of the reference image and we do this by using two loss functions together, the content loss and the style loss.


<p align='center'><img src=./images/TLoss.png></p>
<div align='center'>In the above figure α and β are the weighting factors for content and style reconstruction, respectively.</div> 

<p></p>
The content loss is the L2 norm between the activations of a deeper layer in a pre-trained convnet, computed over the target image, and the activations of the same layer computed over the generated image as shown in the figure below. Here $\vec{p}$ and $\vec{x}$ is the original image and the image that is generated, and P<sup>l</sup> and F<sup>l</sup> their respective feature representations in layer l and F<sub>ij</sub><sup>l</sup> is the activation of the i<sup>th</sup> filter at position j in layer l.

<p align='center'><img src=./images/closs.png></p>
<p></p>

The content loss only uses a single deep layer, but the style loss as defined by Gatys uses multiple layers of a convnet. He used the <i>Gram matrix</i> of a layer’s activations that is the inner product of the feature maps of a given layer. This inner product can be understood as representing a map of the correlations between the layer’s features. By including the feature correlations of multiple layers, we obtain a stationary, multi-scale representation of the input image, which captures its texture information but not the global arrangement. Now the style loss is computed as the L2 norm between the Gram matrices from the original image and the Gram matrices of the image to be generated. In the figure shown below $\vec{a}$  and $\vec{x}$  be the original image and the image that is
generated, and A<sup>l</sup> and G<sup>l</sup> their respective style representation in layer l. The contribution of layer l to the total loss is given below by E<sup>l</sup> and the total style loss by L<sub>style</sub>. Here N<sup>l</sup> is the number of feature maps each of size M<sup>l</sup> for the given layer l and w<sup>l</sup> are weighting factors of the contribution of each layer to the total loss.

<p align='center'><img src=./images/sloss1.png></p>

The original paper was implemented using VGG-19 pretrained on imagenet here I shall implement the same using PyTorch. VGG-19 model architecture is shown below.

<p align='center'><img src=./images/VGG-19.png></p>
