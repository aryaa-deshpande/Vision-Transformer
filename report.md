# A1 - Patch Size
The patch size has an effect on both the accuracy and the speed. The three patch sized tested here were Patch Size 2, Patch Size 4 and Patch Size 8. Using patch size 2 produced the highest final validation accuracy at 48.69% (patch_2.json), compared to 47.99% for patch size 4(patch_4.json) and 39.58% for patch size 8 (patch_8.json). What I did notice was that, the accuracy gain from patch 4 to patch 2 was very small, less than 1% but the computational cost increased by a lot, with patch 2 taking approximately 21 seconds per epoch and patch 4 taking about 5 seconds per epoch. The large accuracy drop for patch 8 makes sense considering it has only 16 patches. The results show that patch 4 would be the perfect balance between accuracy and efficiency for this dataset.

| Patch Size | Val Accuracy | Parameters | Epoch Time  |
|------------|--------------|------------|-------------|
| 2          | 48.69%       | 152,010    | ~21s        |
| 4          | 47.99%       | 142,026    | ~5s         |
| 8          | 39.58%       | 148,170    | ~3.8s       |


# A2 - Number of Layers
This was tested out with three layer options for the model; 2 layers, 4 layers and 8 layers. I noticed that having more number of layers did not help the accuracy, infact it decreased as more layers were added. This is probably because the dataset is so small. The 2 layer model had an accuracy of 49.30% (layer_2.josn), the 4 layer model had an accuracy of 47.99% (layer_4.json) and the 8 layer model had an accuracy of 47.45% (layer_8.json). This most definately happened because our training data was just 5000 images, so an 8 layer model trying to learn on this just ended up overfitting on the data. This can also be comfirmed by taking a look at the number of parameters, the 8 layer model had 275,914 parameters vs the 2 layer model had 75,082 parameters, all for the same small dataset. 

| Layers     | Val Accuracy | Parameters | Train Loss at Epoch 20  |
|------------|--------------|------------|-------------------------|
| 2          | 49.30%       | 75,082     | 0.7963                  |
| 4          | 47.99%       | 142,026    | 0.5584                  |
| 8          | 47.45%       | 275,914    | 0.5063                  |

I specifically included the training loss at epoch 20 because it shows that the 8 layer model has the lowest training loss but actually also has the worst validation accuracy, which are clear signs of overfitting. 

# A3 - Positional Encoding
Since transformers dont inherently understand positions since they parallely process information. This is why it is important to include positional encodings. This experiment was done to see the effect that positional encoding and not having positional enconding had on the performance. On performing the experiment, I noticed that having positional encodings definately did have an improved performance but it was not as big as I thought it would be. There was about a 3% drop in the validation accuracy on removing the positional encoding; with the baseline model with positional encoding having a validation accuracy of 47.99% (patch_4.json) and the model without positional encoding having a validation accuracy of 44.87% (no_pos_enc.json). So the model did learn well even wihtout positional encodings but did have a slightly better performance with positional encodngs present. 

| Config | Val Accuracy | Train Loss at Epoch 20 | Parameters |
|--------|--------------|------------------------|------------|
| With Positional Encoding | 47.99% | 0.5584 | 142,026 |
| Without Positonal Encoding | 44.87% | 0.7444 | 137,866 |

# B1 - Attention Entropy
Looking at my results, I noticed that attention entropy actually increased with depth rather than decreasing. The values went from 5.676 in layer 0 up to 5.999 in layer 3 (attention_entropy.json). This means that the attention became more spread out by the final layer instead of becoming more focused. This is probably became of the small size of the dataset, so the model did not have enough examples to really learn what to focus on and therefore it probably just spread its attention on everything equally.

| Layer | Attention Entropy |
|-------|-------------------|
| 0 | 5.676 |
| 1 | 5.717 |
| 2 | 5.887 | 
| 3 | 5.999 |

# B2 - Positional Embedding Correlation 
Postional embeddings are used by patches to figure out where they are present within the image. Positional Embedding Correlation is measured to check whether patches that are close in the image also have similar positional embeddings. The results show that the model did learn positional embeddings with a Pearson r of -0.3527 (pos_embed_correlation.json). While a perfect score would have been a -1.0, the score of -0.3527 that I got was moderate. This is probably because the model was trained on a small dataset which wasn't enough for the model to develop a strong spatial structure. 

| Metric | Value |
|--------|-------|
| Pearson r | -0.3527 |
| Number of pairs | 2016 |

# B3 - Per Class Accuracy & Confusions
The per-class accuracy show that the model actually performed very differentlyacross the various classes. 
My results show that Ships and Frogs were the easiest to classify, probably because they have very distinct features, like the water around ships and the vibrant green colour for frogs. The hardest to classify were cat and bird. Coming to the confusions pairs, I noticed that the model had missclassifies cats as dogs about 271 times, trucks were being misclassified as automobiles 222 times and dogs as cats 181 times. This mostly makes sense, because dogs and cats have pretty similar features, and even with trucks and automobiles, their shapes are also pretty similar. 

### Per-Class Accuracy
| Class | Accuracy |
|-------|----------|
| Airplane (0) | 52.5% |
| Automobile (1) | 57.6% |
| Bird (2) | 32.4% |
| Cat (3) | 31.8% |
| Deer (4) | 37.4% |
| Dog (5) | 44.8% |
| Frog (6) | 58.6% |
| Horse (7) | 50.6% |
| Ship (8) | 63.3% |
| Truck (9) | 50.9% |

### Confusion Pairs
| True Class | Predicted Class | Count |
|------------|-----------------|-------|
| Cat (3) | Dog (5) | 271 |
| Truck (9) | Automobile (1) | 222 |
| Dog (5) | Cat (3) | 181 |

# B4 - Mean Attention Distance
The mean attention meausres how far a patch attends to, so a small mean distance means that the patch is looking at its close neighbours and a large mean distance means that the patch is essentially looking at the entire image. The results I got show that the attention range mostly grew with depth with layer 0 having a mean attention distance of 4.011 to layer 2 havig a mean attention distance of 4.194, but fell slightly for the last layer having a mean attention distance of 4.147 (attention_distance.json). This means that the earlier layers started with attending nearby patches adn gradually grew to look at more distant patches. 

| Layer | Mean Attention Distance |
|-------|-------------------------|
| 0 | 4.011 |
| 1 | 4.052 |
| 2 | 4.194 |
| 3 | 4.147 |

