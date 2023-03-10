# π μμ¬ νλΈ μΈκ³΅μ§λ₯ μ€νμμ€ κ²½μ§λν π
<img width="60%" src="https://user-images.githubusercontent.com/97013710/210364441-89d27d3f-e22e-4156-ad14-b1a73665dd46.jpeg">
λ§μ λμ κ΄μΈμ΄ λμ΄ μΈμμ λ°λΌλ³΄μ! 

<br/><br/>

## Team

**Name** : λ§μ λμ κ΄μΈ

**Members** : KUBIG 15κΈ° μΌμ€μ, μ₯μν, μ΅λ―Όκ²½ / 16κΈ° λ°λ―Όκ·

<br/><br/>

## Project Descriptions

**Link** : [AI μμ¬ νλΈ μΈκ³΅μ§λ₯ μ€νμμ€ κ²½μ§λν (DACON)](https://dacon.io/competitions/official/235977/overview/description)

**[μ£Όμ ]**  
μ΄λ―Έμ§ μ΄ν΄μν(Image Sper-Resolution)λ₯Ό μν AI μκ³ λ¦¬μ¦ κ°λ°  

**[λ°°κ²½]**  
μ€ν μμ€ μ΄λ―Έμ§ λ°μ΄ν°λ₯Ό νμ©νμ¬ μΈκ³΅μ§λ₯ μ»΄ν¨ν° λΉμ μ 'μ΄λ―Έμ§ μ΄ν΄μν' λΆμΌ μ°κ΅¬κ°λ°  

**[μ€λͺ]**  
νμ§μ΄ μ νλ μ ν΄μλ μ΄¬μ μ΄λ―Έμ§(512x512)λ₯Ό κ³ νμ§μ κ³ ν΄μλ μ΄¬μ μ΄λ―Έμ§(2048x2048)λ‘ μμ±  

**[νκ° μ°μ]**  
PSNR(Peak Signal-to-Noise Ratio) = $10log_{10}(R^2/MSE)$
-	μμ± νΉμ μμΆλ μμμ νμ§μ λν βμμ€ μ λ³΄βλ₯Ό νκ°
-	μμ€μ΄ μ μμλ‘(=νμ§μ΄ μ’μ μλ‘) λμ κ°  

<br/><br/>

## π£ Score(Public)
RRDB+ : 23.40812(38th)

<br/><br/>

## π Environment
Colab Pro+  
GPU: A100-SXM4-40GB * 1(Main) , Tesla T4*1(Sub)

<br/><br/>

## π₯ Competition Strategies

**1. Patches(for data augmentation)**  
- Train patches : original 1640 images β 26240(1640*16) patches (X4 downsampling, non-overlapping)  
- Test patches: original 18 images β 882(18*49) patches (X4 downsampling, overlapping(to remove border artifacts)) 

<br/>

**2. Data Transform**  
Non-destructive transformations (not to add or lose the information)
- Flip  
- Transpose  
- RandomRotate  
- ShiftScaleRotate  

<br/>

**3. Training Methods**
- EarlyStopping  
  > To prevent overfitting  
  > If validation loss does not improve after given patience(=2), training is earlystopped  
- Fine-tuning with pre-trained model  
  > pretrained model : [RRDB_PSNR_x4.pth](https://github.com/xinntao/ESRGAN/tree/master/models)(the PSNR-oriented model withΒ high PSNR performance)  
  > Retraining entire model : Judging that the similarity between DF2K dataset(pretrained model) and our training datset is small  

<br/>

**4. Loss Function**
- L1 loss + L2 loss (2:1)
  > L2 loss : PSNR is based on MSE  
  > L1 loss: For better convergence [https://arxiv.org/pdf/1707.02921v1.pdf](https://arxiv.org/pdf/1707.02921v1.pdf)

<br/>

**5. Learning Scheduler, Optimizer**
- StepLR  
  > step_size = 3, gamma = 0.5  
  > Decays the learning rate of each parameter in half once per 3 epochs 
- Adam  

<br/>

**6. Post Processing**
- Geometric Self-Ensemble [https://arxiv.org/pdf/1707.02921v1.pdf](https://arxiv.org/pdf/1707.02921v1.pdf)
  > <img width="751" alt="KakaoTalk_Photo_2023-01-04-11-58-49" src="https://user-images.githubusercontent.com/55012723/210476203-015eac00-d0e0-4d10-8eb5-a772a9910097.png">


<br/><br/> 

## Main configuration & Hyperparameters
'''
1. Manuel_seed : 42  

2. Model :  
   > num_feat : 64 , Channel number of intermediate features.  
   > growth_channel: 32 , Channels for each growth(dense connection).  
   > num_block: 23 , number of RRDB blocks.  

3. Dataloader :  
   > train_batch_size : 4  
   > test_batch_size: 1  
   > num_workers: 4   

4. Train :  
   > epochs: 7  
   > optim_g: {type: Adam, lr: 1e-4, betas: [0.9, 0.99]}  

'''

<br/><br/>

## Code Descriptions
1. DACON_AISR_TRIAL
- EDSR, SRGAN, SWINIR


2. DACON_AISR_BEST
- RRDB, RRDB+(Self-ensemble)
