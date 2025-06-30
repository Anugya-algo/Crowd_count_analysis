# Crowd_count_analysis

MAIN MODEL:  Built a custom architecture using:
Pretrained VGG16 frontend
A dual-branch decoder to capture varied receptive fields (3×3 and 5×5)
CBAM modules after encoding and decoding to focus the network’s attention spatially and channel-wise
A patch-based training strategy (512×512) to efficiently train on large images and capture local density variations
This design allowed us to extract robust features, adapt to scale variations, and produce high-fidelity density maps while maintaining memory efficiency.     
  

1. CSRNet (Congested Scene Recognition Network)
Type: CNN-based
Description: Uses a truncated VGG-16 frontend and dilated convolutions in the backend to maintain spatial resolution while expanding receptive field.
Strengths: Simple yet effective; performs well on dense crowds.
Variants You Tried:
Replacing backend with custom dilation layers.
Adding attention modules like CBAM.
Reference: CSRNet Paper

2. MCNN (Multi-Column Convolutional Neural Network)
Type: CNN with multi-branch
Description: Uses three parallel convolutional branches with different kernel sizes to capture multi-scale crowd features.
Strengths: Good on varying crowd densities and scales.
Limitations: Less precise than newer models on very dense scenes.

 3. CBAM-Enhanced Custom CNN
Type: Custom CNN + Attention
Description:
Sequential CNN layers (Conv2D → ReLU)
Followed by a CBAMLayer (Channel + Spatial Attention)
Use: Focus on relevant crowd regions; efficient and lightweight.

 4. Multi-Dilated CNN + CBAM 
         x
       / | \
     rate=1,2,3 → CNN stacks → concatenate → CBAM → 1x1 Conv → Density map
Type: Multi-scale + Attention
Description:
Three parallel branches with dilation rates 1, 2, and 3
Merged output passed through CBAM attention
Final 1x1 Conv layer for density prediction
Strengths:
Combines wide receptive fields
Learns important regions via attention
Your Innovation: Dilation + CBAM combo not common in crowd models

5. Dilated Convolutional Variants
Type: Variants of CSRNet
Description: Replaced backend with:
Higher dilation rates
Different configurations (e.g., fixed vs varying rates)
Goal: Expand receptive field without pooling; preserve resolution

 6. CBAM Layer Module (Standalone)
Type: Module (not full model)
Description: Implemented CBAM manually:
Channel attention via Dense layers on pooled features
Spatial attention via 7×7 conv over concatenated avg/max maps
Used in: Integrated into multi-dilation models and plain CNNs

 7. CCTrans (Crowd Counting Transformer) — Explored/Analyzed
Type: Transformer-based crowd counting
Description: Uses CNN encoder + transformer modules for global context
Status: You’ve analyzed the architecture and asked about merging MCNN into it — not fully implemented yet.

8.Transposed convolutions (also known as deconvolutions or fractionally strided convolutions) were incorporated in the
intermediate layers of the model architecture to perform upsampling. This approach helps the network to learn spatial
representations at a higher resolution, preserving spatial details. By integrating these layers mid-architecture, 
we aimed to recover finer features lost during earlier downsampling stages, thereby improving overall performance.

