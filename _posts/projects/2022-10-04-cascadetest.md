---
layout: post
title: 
description: |
  Swin Transformer for Object Detection - Trainig with COCO dataset
hide_image: true
tags:
  - projects
published: true
---
```
root@42fbee0e8ce8:~/mmdetection# python tools/test.py configs/swin/cascade_mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco.py work_dirs/cascade_mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco/latest.pth --eval bbox segm
/root/mmdetection/mmdet/utils/setup_env.py:39: UserWarning: Setting OMP_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
  f'Setting OMP_NUM_THREADS environment variable for each process '
/root/mmdetection/mmdet/utils/setup_env.py:49: UserWarning: Setting MKL_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
  f'Setting MKL_NUM_THREADS environment variable for each process '
loading annotations into memory...
Done (t=0.81s)
creating index...
index created!
load checkpoint from local path: work_dirs/cascade_mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco/latest.pth
[                                                  ] 0/5000, elapsed: 0s, ETA:/opt/conda/lib/python3.7/site-packages/torch/nn/functional.py:718: UserWarning: Named tensors and all their associated APIs are an experimental feature and subject to change. Please do not use them for anything important until they are released as stable. (Triggered internally at  /opt/conda/conda-bld/pytorch_1623448265233/work/c10/core/TensorImpl.h:1156.)
  return torch.max_pool2d(input, kernel_size, stride, padding, dilation, ceil_mode)
[>>] 5000/5000, 6.8 task/s, elapsed: 740s, ETA:     0s
Evaluating bbox...
Loading and preparing results...
DONE (t=0.55s)
creating index...
index created!
Running per image evaluation...
Evaluate annotation type *bbox*
DONE (t=33.17s).
Accumulating evaluation results...
DONE (t=6.41s).

 Average Precision  (AP) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.508
 Average Precision  (AP) @[ IoU=0.50      | area=   all | maxDets=1000 ] = 0.701
 Average Precision  (AP) @[ IoU=0.75      | area=   all | maxDets=1000 ] = 0.555
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= small | maxDets=1000 ] = 0.335
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=medium | maxDets=1000 ] = 0.542
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= large | maxDets=1000 ] = 0.663
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.630
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=300 ] = 0.630
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=1000 ] = 0.630
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= small | maxDets=1000 ] = 0.455
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=medium | maxDets=1000 ] = 0.664
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= large | maxDets=1000 ] = 0.779


Evaluating segm...
/root/mmdetection/mmdet/datasets/coco.py:474: UserWarning: The key "bbox" is deleted for more accurate mask AP of small/medium/large instances since v2.12.0. This does not change the overall mAP calculation.
  UserWarning)
Loading and preparing results...
DONE (t=1.42s)
creating index...
index created!
Running per image evaluation...
Evaluate annotation type *segm*
DONE (t=37.50s).
Accumulating evaluation results...
/opt/conda/lib/python3.7/site-packages/pycocotools/cocoeval.py:378: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
  tp_sum = np.cumsum(tps, axis=1).astype(dtype=np.float)
DONE (t=6.47s).

 Average Precision  (AP) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.443
 Average Precision  (AP) @[ IoU=0.50      | area=   all | maxDets=1000 ] = 0.675
 Average Precision  (AP) @[ IoU=0.75      | area=   all | maxDets=1000 ] = 0.480
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= small | maxDets=1000 ] = 0.245
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=medium | maxDets=1000 ] = 0.474
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= large | maxDets=1000 ] = 0.636
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.555
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=300 ] = 0.555
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=1000 ] = 0.555
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= small | maxDets=1000 ] = 0.370
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=medium | maxDets=1000 ] = 0.594
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= large | maxDets=1000 ] = 0.716

OrderedDict([('bbox_mAP', 0.508), ('bbox_mAP_50', 0.701), ('bbox_mAP_75', 0.555), ('bbox_mAP_s', 0.335), ('bbox_mAP_m', 0.542), ('bbox_mAP_l', 0.663), ('bbox_mAP_copypaste', '0.508 0.701 0.555 0.335 0.542 0.663'), ('segm_mAP', 0.443), ('segm_mAP_50', 0.675), ('segm_mAP_75', 0.48), ('segm_mAP_s', 0.245), ('segm_mAP_m', 0.474), ('segm_mAP_l', 0.636), ('segm_mAP_copypaste', '0.443 0.675 0.480 0.245 0.474 0.636')])
```
