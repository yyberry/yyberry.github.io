---
layout: post
title: Pytorch - save and load model
subtitle: This is the learning notes for Pytorch
# cover-img: /assets/img/cover1.png
# thumbnail-img: /assets/img/thumbnail1.jpeg
# share-img: /assets/img/cover1.png
tags: [PyTorch, Deep Learning]
---

This is a learning note related to the PyTorvh. It contains the basic introduction to how to save and load the trained model.


### Save

``torch.save(obj, PATH)`` where `obj` could be models, tensors or dictionaries.

Use ``torch.save(model, 'save.pt')``  to save the whole model 

Or

Use ``torch.save(model.state_dic())`` to save trained weights with faster speed and less space.



### Load

Before load the model, the model should be defined first.

Then correspondingly

Use ``torch.load(PATH)``  to load the whole model

Or 

Use ``model.load_state_dic(torch.load(PATH))`` to load trained weights.