# npu-qwen3-8b
硬件环境为310P3, 下载的镜像为2.0.RC2-300I-Duo-py311-openeuler24.03-lts
docker run -itd --net=host --shm-size=10g --privileged --name mindie_qw \
	--device=/dev/davinci_manager --device=/dev/hisi_hdc --device=/dev/devmm_svm \
	-v /usr/local/Ascend/driver:/usr/local/Ascend/driver:ro \
	-v /usr/local/sbin:/usr/local/sbin:ro \
	-v /path-to-weights:/path-to-weights:ro \
	-v ***/Qwen3-8B:/PTM/Qwen3-8B \
	-v ***/config2.json:/usr/local/Ascend/mindie/latest/mindie-service/conf/config.json \
	swr.cn-south-1.myhuaweicloud.com/ascendhub/mindie:2.0.RC2-300I-Duo-py311-openeuler24.03-lts \
	/bin/bash
 
根据https://www.hiascend.com/document/detail/zh/mindie/20RC2/envdeployment/instg/mindie_instg_0026.html

1.设置
chmod 750 mindie-service
chmod -R 550 mindie-service/bin
chmod -R 500 mindie-service/bin/mindie_llm_backend_connector
chmod 550 mindie-service/lib
chmod 440 mindie-service/lib/*
chmod 550 mindie-service/lib/grpc
chmod 440 mindie-service/lib/grpc/*
chmod -R 550 mindie-service/include
chmod -R 550 mindie-service/scripts
chmod 750 mindie-service/logs
chmod 750 mindie-service/conf
chmod 640 mindie-service/conf/config.json
chmod 700 mindie-service/security
chmod -R 700 mindie-service/security/*

2.设置
chmod +w set_env.sh 
source /usr/local/Ascend/ascend-toolkit/set_env.sh                                 # CANN
source /usr/local/Ascend/nnal/atb/set_env.sh                                       # ATB
source /usr/local/Ascend/atb-models/set_env.sh                                # ATB Models
source mindie-service/set_env.sh

3.修改/PTM/Qwen3-8B config文件权限

4.pip更新transformers 4.51.0

5.将【/usr/local/Ascend/atb-models】改名，这里改为【atb-models.bak】
将附件【atb-models.tar.gz】解压，把解压后的【atb-models】复制到【/usr/local/Ascend/】

6. ./bin/mindieservice_daemon
启动后可以通过访问得到结果
curl -X POST http://ip:port/v1/chat/completions ^
-H "Content-Type: application/json" ^
-d "@data.json"
curl -X GET http://192.168.101.105:1032/v1/models 
