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
2022-10-03 07:04:53,048 - mmdet - INFO - Evaluating bbox...
Loading and preparing results...
DONE (t=0.26s)
creating index...
index created!
Running per image evaluation...
Evaluate annotation type *bbox*
DONE (t=34.01s).
Accumulating evaluation results...
DONE (t=6.51s).
2022-10-03 07:05:34,436 - mmdet - INFO - 
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

2022-10-03 07:05:34,437 - mmdet - INFO - Evaluating segm...
/root/mmdetection/mmdet/datasets/coco.py:474: UserWarning: The key "bbox" is deleted for more accurate mask AP of small/medium/large instances since v2.12.0. This does not change the overall mAP calculation.
  UserWarning)
Loading and preparing results...
DONE (t=1.44s)
creating index...
index created!
Running per image evaluation...
Evaluate annotation type *segm*
DONE (t=39.07s).
Accumulating evaluation results...
/opt/conda/lib/python3.7/site-packages/pycocotools/cocoeval.py:378: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
  tp_sum = np.cumsum(tps, axis=1).astype(dtype=np.float)
DONE (t=6.49s).
2022-10-03 07:06:22,987 - mmdet - INFO - 
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

2022-10-03 07:06:23,610 - mmdet - INFO - Exp name: cascade_mask_rcnn_swin-s-p4-w7_fpn_fp16_ms-crop-3x_coco.py
2022-10-03 07:06:23,610 - mmdet - INFO - Epoch(val) [36][1250]  bbox_mAP: 0.5080, bbox_mAP_50: 0.7010, bbox_mAP_75: 0.5550, bbox_mAP_s: 0.3350, bbox_mAP_m: 0.5420, bbox_mAP_l: 0.6630, bbox_mAP_copypaste: 0.508 0.701 0.555 0.335 0.542 0.663, segm_mAP: 0.4430, segm_mAP_50: 0.6750, segm_mAP_75: 0.4800, segm_mAP_s: 0.2450, segm_mAP_m: 0.4740, segm_mAP_l: 0.6360, segm_mAP_copypaste: 0.443 0.675 0.480 0.245 0.474 0.636
INFO:torch.distributed.elastic.agent.server.api:[default] worker group successfully finished. Waiting 300 seconds for other agents to finish.
INFO:torch.distributed.elastic.agent.server.api:Local worker group finished (SUCCEEDED). Waiting 300 seconds for other agents to finish
/opt/conda/lib/python3.7/site-packages/torch/distributed/elastic/utils/store.py:71: FutureWarning: This is an experimental API and will be changed in future.
  "This is an experimental API and will be changed in future.", FutureWarning
INFO:torch.distributed.elastic.agent.server.api:Done waiting for other agents. Elapsed: 0.0013725757598876953 seconds
{"name": "torchelastic.worker.status.SUCCEEDED", "source": "WORKER", "timestamp": 0, "metadata": {"run_id": "none", "global_rank": 0, "group_rank": 0, "worker_id": "93895", "role": "default", "hostname": "42fbee0e8ce8", "state": "SUCCEEDED", "total_run_time": 327467, "rdzv_backend": "static", "raw_error": null, "metadata": "{\"group_world_size\": 1, \"entry_point\": \"python\", \"local_rank\": [0], \"role_rank\": [0], \"role_world_size\": [4]}", "agent_restarts": 0}}
{"name": "torchelastic.worker.status.SUCCEEDED", "source": "WORKER", "timestamp": 0, "metadata": {"run_id": "none", "global_rank": 1, "group_rank": 0, "worker_id": "93896", "role": "default", "hostname": "42fbee0e8ce8", "state": "SUCCEEDED", "total_run_time": 327467, "rdzv_backend": "static", "raw_error": null, "metadata": "{\"group_world_size\": 1, \"entry_point\": \"python\", \"local_rank\": [1], \"role_rank\": [1], \"role_world_size\": [4]}", "agent_restarts": 0}}
{"name": "torchelastic.worker.status.SUCCEEDED", "source": "WORKER", "timestamp": 0, "metadata": {"run_id": "none", "global_rank": 2, "group_rank": 0, "worker_id": "93897", "role": "default", "hostname": "42fbee0e8ce8", "state": "SUCCEEDED", "total_run_time": 327467, "rdzv_backend": "static", "raw_error": null, "metadata": "{\"group_world_size\": 1, \"entry_point\": \"python\", \"local_rank\": [2], \"role_rank\": [2], \"role_world_size\": [4]}", "agent_restarts": 0}}
{"name": "torchelastic.worker.status.SUCCEEDED", "source": "WORKER", "timestamp": 0, "metadata": {"run_id": "none", "global_rank": 3, "group_rank": 0, "worker_id": "93898", "role": "default", "hostname": "42fbee0e8ce8", "state": "SUCCEEDED", "total_run_time": 327467, "rdzv_backend": "static", "raw_error": null, "metadata": "{\"group_world_size\": 1, \"entry_point\": \"python\", \"local_rank\": [3], \"role_rank\": [3], \"role_world_size\": [4]}", "agent_restarts": 0}}
{"name": "torchelastic.worker.status.SUCCEEDED", "source": "AGENT", "timestamp": 0, "metadata": {"run_id": "none", "global_rank": null, "group_rank": 0, "worker_id": null, "role": "default", "hostname": "42fbee0e8ce8", "state": "SUCCEEDED", "total_run_time": 327467, "rdzv_backend": "static", "raw_error": null, "metadata": "{\"group_world_size\": 1, \"entry_point\": \"python\"}", "agent_restarts": 0}}
```
