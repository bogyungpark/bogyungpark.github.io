---
title: [Ubuntu]GPU initial Settings
by: bogyung park
date: 2023-07-07 00:00:00 +0800
categories: [Ubuntu, GPU initial Settings]
tags: [GPU initial Settings]
---

# Ubuntu initial Settings

## Driver installation
1. 그래픽카드 정보 및 드라이버 확인하기
```
$ ubuntu-drivers devices
```    
![image](/image/first_post/drivercheck.png)
2. 드라이버 설치
    - 권장 드라이버 자동으로 설치
    ```
	    $ sudo ubuntu-drivers autoinstall 
	    $ sudo reboot
    ```
	-  원하는 버전 수동으로 설치
    ```
		$ sudo apt install nvidia-driver-470 
	    $ sudo reboot
    ```

## Monitoring GPU usage.
```
    $ nvidia-smi
``` 
![image](/image/first_post/usage.png)
-  **Driver version**
    
    : 현재 설치돼 있는 nvidia driver version
    
- **CUDA Version**
    
    : 현재 드라이버 버전과 맞는 cuda 추천 버전
    
- **GPU/FAN**
    
    : 설치되어 있는 GPU number: Fan이 있는 RTX 모델
    
- **Name**
    
    : GeForce RTX 3090
    
- **Temp**
    
    : GeForce RTX 3090 모델
    
- **Perf (Performance)**
    
    : P0 - P12 까지 존재하며, P0에 가까울수록 GPU의 Performance가 높다
    
- **Persistence-M**
    
    : Power 지속성 옵션은 on/off 두 가지 모드가 존재하며, Default 값은 off이다. on 상태가 되면 power limit을 설정할 수 있다.

    
- **Pwr: Usage/Cap**
    
    : 현재 전력 사용량과 최대 용량
    
- **Bus-Id**
    
    : 서버 제조사의 메인보드마다 가지고 있는 PCI slot에 부여된 BUS-ID 사용하는 GPU number와 메인보드의 PCI 슬롯을 매칭 시켜 확인할 수 있다.
    
- **Disp.A**
    
    : RTX나 Quadro계열의 GPU에서 사용. 모니터를 연결한 출력 포트의 GPU는 on상태로 변경이 된다.
    
- **Memory-Usage**
    
    : 현재 사용하는 GPU 메모리 / 총 GPU 메모리
    
- **Voltaile GPU-Util**
    
    : GPU의 총사용량

- **Uncorr. ECC**
    
    :** default값은 ECC ON 상태이며, ECC count가 생기면 숫자 0이 1,2.. 이상으로 변경
    
- **Compute M.**
    
    : Compute Mode의 모드
    
    0 - default
    1 - exclusive_thread
    2 - prohibited
    3 - exclusive_process
    
- **MIG M**
    
    : MIG는 GPU를 Slice 하는 기능
    
- **Processes**
    
    : GPU가 작업을 시작하면 No running processes found에 PID에 표기


## **CUDA Toolkit Install**

The driver and toolkit must be installed for CUDA to function. If you have not installed a stand-alone driver, install the driver from the NVIDIA CUDA Toolkit.

→ 쿠다를 실행하기 위해서는 driver와 toolkit가 깔려 있어야 한다. 단독 Nvidia driver를 깔지 않았다면, CUDA Toolkit을 통해 깔도록 한다.

 [](http://developer.nvidia.com/cuda-downloads)[CUDA Toolkit Archive사이트](https://developer.nvidia.com/cuda-toolkit-archive)와 [Cudnn 설치 URL](http://developer.nvidia.com/rdp/cudnn-download)를 통해 설치할 수 있다.

1. [CUDA Toolkit 설치 URL](http://developer.nvidia.com/cuda-downloads)에 들어가 환경에 맞는 버전을 선택
2. Base Installer에 있는 명령어들을 차례로 입력하여 설치
![image](/image/first_post/install_1.png)
![image](/image/first_post/install_2.png)

```
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
$ sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda-repo-ubuntu2004-12-2-local_12.2.0-535.54.03-1_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu2004-12-2-local_12.2.0-535.54.03-1_amd64.deb
$ sudo cp /var/cuda-repo-ubuntu2004-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/
$ sudo apt-get update
$ sudo apt-get -y install cuda
```

```
$ sudo apt update
$ sudo apt install nvidia-cuda-toolkit
$ nvcc -V
```
![image](/image/first_post/nvcc.png)

## **Pytorch GPU Install**
[PyTorch Get start](https://pytorch.org/get-started/locally/)에 접속한다.
![image](/image/first_post/pytorch.png)
```
$ conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
```

### Pytorch GPU 사용 가능 여부 확인
```
import torch

print(torch.cuda.is_available())
```
GPU 사용 가능 -> True, GPU 사용 불가 -> False