# Stable Diffusion 미세조정 실습 (SD 1.5 DreamBooth LoRA)

내 물건 사진 몇 장으로 Stable Diffusion 1.5에 LoRA를 학습하고, 같은 프롬프트와 seed로 네 방법을 비교하는 실습이다. 무료 T4 GPU에서 20~30분 걸린다.

| 방법 | 학습하는 것 | 이 실습에서 |
|---|---|---|
| 베이스 | 없음 | 비교 기준 |
| **DreamBooth LoRA** | UNet에 붙인 작은 어댑터 | **직접 학습** |
| Textual Inversion | 새 단어 하나의 임베딩 벡터 | 미리 학습한 임베딩으로 추론 |
| IP-Adapter | 없음 (참조 사진 한 장) | 추론 |

## 시작하기

### Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/munain0417/sd-finetune-lab/blob/main/01_student_lora_sd15.ipynb)

1. 위 버튼으로 노트북을 연다.
2. 메뉴 `런타임 > 런타임 유형 변경 > T4 GPU`를 고른다.
3. 위에서 아래로 셀을 차례로 실행한다.

### Kaggle

1. `Create > New Notebook`에서 `File > Import Notebook`을 고르고 이 저장소의 `01_student_lora_sd15.ipynb`를 가져온다 (GitHub 주소 또는 파일 업로드).
2. 오른쪽 `Settings`에서 `Accelerator`를 `GPU T4`로, `Internet`을 켠다 (전화번호 인증 필요).
3. 위에서 아래로 셀을 차례로 실행한다.

Hugging Face 토큰은 필요 없다.

## 학습 대상

노트북의 `SUBJECT`로 고른다.

- **샘플 대상** (`can`, `cat2`, `monster_toy`): 노트북이 [google/dreambooth](https://github.com/google/dreambooth) 데이터셋에서 고른 대상의 사진만 받는다.
- **내 사진** (`mine`): 같은 물건을 배경과 각도를 다양하게 해서 10~20장 찍어 올린다. 배경이 모두 같으면 모델이 물건과 함께 배경까지 외운다.

## 미리 학습한 가중치

학습을 못 했을 때 쓰는 비상용 LoRA와, 비교에 쓰는 Textual Inversion 임베딩은 Hugging Face Hub에서 받는다.

- [munain0417/sd15-practice-lora](https://huggingface.co/munain0417/sd15-practice-lora): 샘플 세 대상의 LoRA
- [munain0417/sd15-practice-ti](https://huggingface.co/munain0417/sd15-practice-ti): 샘플 세 대상의 TI 임베딩

## 사용한 것

- 베이스 모델: [stable-diffusion-v1-5/stable-diffusion-v1-5](https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-v1-5)
- 학습 스크립트: diffusers v0.42.1의 [`train_dreambooth_lora.py`](https://github.com/huggingface/diffusers/blob/v0.42.1/examples/dreambooth/train_dreambooth_lora.py)
- IP-Adapter: [h94/IP-Adapter](https://huggingface.co/h94/IP-Adapter)
- 샘플 데이터: [google/dreambooth](https://github.com/google/dreambooth). 사진별 출처와 라이선스는 원본의 `dataset/references_and_licenses.txt`를 따른다.

```bibtex
@inproceedings{ruiz2023dreambooth,
  title={Dreambooth: Fine tuning text-to-image diffusion models for subject-driven generation},
  author={Ruiz, Nataniel and Li, Yuanzhen and Jampani, Varun and Pritch, Yael and Rubinstein, Michael and Aberman, Kfir},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  year={2023}
}
```
