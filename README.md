# Auto-Encoding Scene Graphs for Image Captioning [Yang, et al '19]

Graph R-CNN :  pytorch 1.0 버전에서만 동작

venv -> conda activate graph_rcnn

SGAE :  pytorch 1.5 버전에서만 동작 

vcnv -> conda activate sgae


# 코드 

cider-master    : score 측정

coco-caption    : score 측정 

graph_rcnn      : scene graph 생성

models          : 모델

scripts         : 스크립트 파일들 

# 실행 방법 

* Graph R-CNN (Scene graph 생성) \
    
    1. 데이터 셋 \
    
        /SGAE-final/graph_rcnn/data_tools/data
        
        MS COCO 이미지 데이터  \
        [MS COCO](https://cocodataset.org/#download)
        ```
        train2014/
        val2014/
        test2014/
        ```
        [Split MetaData](https://drive.google.com/drive/folders/1GvwpchUnfqUjvlpWTYbmEvhvkJTIWWRb)
        ```
        cocobu2.json
        ```
       [Vocab data](https://github.com/danfeiX/scene-graph-TF-release/tree/master/data_tools)
       ```
        VG-SGG-dict.json
        VG_SGG.h5
        mv VG-SGG-dict.json ../../datasets/vg_bm
        mv VG_SGG.h5 ../../datasets/vg_bm
       ``` 
    
    2. 이미지 전처리
       ```
       cd ..
       bash ./create_imdb.sh
       ```
       결과 파일 (**COCO_imdb_1024.h5**) 생성 
       
       Directory 이동 
       ```
        mv data_tools/VG/COCO_imdb_1024.h5 datasets/vg_bm
       ```
    
    3. Scene Graph 생성
        1) ** Compile **
        ```
        cd graph_rcnn/lib/scene_parser/rcnn
        python setup.py build develop
        ```
        2)
        ```
        bash ./eval.sh
        ```
        results 에 graph_rcnn 결과 파일 생성
        
    2-3 번 실행
        /SGAE-final
        bash ./scripts/get_scene_graph.sh
        
        
* SGAE (Image Captioning)\
    [Drive](https://drive.google.com/drive/folders/1W9UTkdkCH5Hk0OLDGoPTECTQ2gcqOQc1?usp=sharing) \
    데이터 \
    ``` data/coco_spice_sg2/ ``` sentence scene graphs \
    ``` data/cocobu2.json```     image metadata \
    ```data/cocobu2_labe.h5 ``` 
    image labels \
    \
        - Sentence Scene graph 생성 -
        
        ```
        create_coco_sg.py
        process_spice_sg.py
        ```
    
    SGAE-final/
    
    1. Graph R-CNN 결과 전처리 
        ```
        bash ./scripts/prepro_graph_rcnn.sh
        ```
       결과 파일 *_att, *_box, *_fc 생성 
       
    2. Train
        ```
        bash ./train.sh
        ```
       
       중간 지점부터 재학습 
       ```
        bash ./train_from.sh
        ```
    
    3. Eval
        ```
        bash ./eval.sh
        ```
