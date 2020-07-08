---
title: "[Code]Part-Based Graph Convolutional Network for Action Recognition(PB-GCN)" ## 포스트 제목
category:       
    - Paper Review
tags:           
    - GCN
comments:  true
use_math : true
last_modified_at : 2020-07-01
toc: true
published: false
---

[Timer]
- dataloader : 데이터 로딩 소요 시간
- model : 모델 학습 소요 시간
- statistics : 추정 소요 시간

[Runner] load_data
- args.dataset : dataset name
- args.phase : train / val / test
- args.loader : custom data loader (NTUDataloader)
- args.train_loader_args : data, label path

[Runner] load_model
- args.device : <list> for multiGPU => self.output_device
- args.model : 'Which do will model be used?', model name? STGCResidual?
- args.model.args : model parameters
    STGCResidual :  channel,
                    num_class,
                    window_size,
                    num_joints,
                    num_actors=1,
                    use_data_bn=False,
                    layers_config=None,
                    graph=None,
                    graph_args=dict(),
                    mask_learning=False,
                    use_local_bn=False,  
                    multiscale=False,
                    temporal_kernel_size=9,
                    dropout=0.5,
                    dataset='NTU'
- args.weights : <str> (default : None) load the model weight for pre-trained model or before trained model
- args.ignore_weights : remove some weights which want to ignore

[Runner] load_optimizer
- args.optimizer : <str> optimizer type
- args.base_lr : <float> base learning rate
- args.weight_decay : <float> weight decay

[Runner] adjust_learning_rate
- args.step : <int> (default=[20,40,60]) the epoch where optimizer reduce the learning rate

[Runner] train
- args.show_topk : <int> (default=[1,5])
- args.log_interval : logging epoch interval

[Runner] run
- args.start_epoch : start epoch
- args.num_epoch : number of epoch
- args.eval_interval : eval interval
- args.save_score : <bool> save score indicator


[Runner] eval
- args.save_path : test score_frag save dir