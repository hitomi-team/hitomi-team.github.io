---
title: "An Image is Worth a Thousand Words"
date: "2022-05-13"
author: "haru"
---

## Zero-Shot Image Captioning
- Allowing Dialog Language Models to approximate the content of an image through Image Captioning with the help of CLIP.

## Overview

We allowed Language Models like GPT-J 6B to be able to have an approximate overview of images by classifying an image with labels using natural language. Our implementation was heavily inspired through Google Research's [Socratic Models](https://socraticmodels.github.io/) which aims to provide multimodal reasoning for Large Language Models such as GPT-3.

![Example](https://i.imgur.com/SxRLMzg.png)

Our Image Captioning system works with multiple categories of labels ranging from fictional characters to what medium the image was created in to what objects are present within the image. With more and more diverse labels to classify an image against, the Image Captioning system would be able to more accurately approximate an image into natural language. It gathers the ``top-k`` labels with the highest probability for all of the label sets when sorted, as erraneous labels would inevitably have the highest probability in some sets.

## Use in Chatbots

Within our chatbots, this system allows us to explore how well of a system such as this would behave with our finetuned conversational language models such as our finetuned [Convo-6B](https://huggingface.co/hitomi-team/convo-6B) model.

![Example 2](https://i.imgur.com/2daXOFw.png)

For our chatbots, the above message would appear like this:

```
haru: @Sakuya Izayoi hey
haru: [Image Attached: Image with the Touhou Project character Tenshi Hinanawi in it.]
```

This style of formatting helps our model know *what* is in an image, even if the language model cannot actually see the image itself.

## Limitations

As mentioned above, this presents itself as a readily available solution for approximated vision with the tools that are presently available. There are numerous limitations with this technique as the only information extracted is very limited compared to what is in the image itself. After all, an image is worth a thousand words, but a more accurate saying for our use case would be ``an image is worth a thousand words that we can't cram into our Language Models or else our local GPUs would catch fire.`` Or, less excitingly, a megabyte long error is produced.

CLIP itself has a number of limitations as well, many of which are explained within their [own blog post](https://openai.com/blog/clip/) and [this one as well](https://stanislavfort.github.io/blog/OpenAI_CLIP_stickers_and_adversarial_examples/). Essentially, CLIP is very prone to adversarial attacks where text within the image can influence the classification of an image according to CLIP.

Here is an example of that happening with our own chat bots where an image containing the phrase ``Alice`` lead to the label ``Alice Margatroid`` having a very high probability and ending up within the context sent to the Language Model:

![Example 3](https://i.imgur.com/tRmDyDL.png)

``[Image Attached:  Image with the Touhou Project character Alice Margatroid in it. This image depicts a amusement park. This is a sketch.``

## Conclusion

Using CLIP to classify an image with labels to approximate for chatbots is a great way to experiment on the limits of these systems without needing to do any heavy lifting ourselves. In order to mitigate against the limitations outlined above, we used several different techniques to prevent the appearance of such things while still allowing the conversation between human and machine to go smoothly.

For now, we will continue exploring this avenue of multimodal conversations with chatbots as it is something that could potentially open doors for future research in other areas outside of just computer science. We hope you enjoyed reading about our experience with Image Captioning with the help of CLIP.

#### Links

- [Discord Server](https://discord.gg/Sx6Spmsgx7)

- [The Code for the Chatbots](https://github.com/harubaru/eliza)

- [Our Backend Software](https://github.com/hitomi-team/sukima)
