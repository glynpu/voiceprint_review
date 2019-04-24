plda / eigenface 是人脸识别的常用方法

[看这篇i-vector 推导详解明白了i-vector EM 的Mstep 怎么算得，但是作者没有给出公式引用文章的出处](https://blog.csdn.net/weixin_38206214/article/details/81541497)

<a href="https://www.codecogs.com/eqnedit.php?latex=\Phi_c&space;=&space;\sum_s\sum_hN_{c,h}(s)E[\omega{s,h}\omega^*_{s,h}],&space;(c&space;=&space;1,&space;...C)&space;\\&space;\Omega&space;=&space;\sum_s\sum_h&space;\tilde{F}_h(s)E[\omega^*_{s,h}]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Phi_c&space;=&space;\sum_s\sum_hN_{c,h}(s)E[\omega{s,h}\omega^*_{s,h}],&space;(c&space;=&space;1,&space;...C)&space;\\&space;\Omega&space;=&space;\sum_s\sum_h&space;\tilde{F}_h(s)E[\omega^*_{s,h}]" title="\Phi_c = \sum_s\sum_hN_{c,h}(s)E[\omega{s,h}\omega^*_{s,h}], (c = 1, ...C) \\ \Omega = \sum_s\sum_h \tilde{F}_h(s)E[\omega^*_{s,h}]" /></a>


<a href="https://www.codecogs.com/eqnedit.php?latex=T_i\Phi_c=\Omega_i,(i=1,...CP)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T_i\Phi_c=\Omega_i,(i=1,...CP)" title="T_i\Phi_c=\Omega_i,(i=1,...CP)" /></a>

这个更新的公式就比较好理解了，<a href="https://www.codecogs.com/eqnedit.php?latex=\Omega_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Omega_i" title="\Omega_i" /></a> 对应的是observation variables,

<a href="https://www.codecogs.com/eqnedit.php?latex=T_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T_i" title="T_i" /></a> 对应的是latent variables

Observation variables 一直都知道， latent variables是通过E step求解出来的，在进行M-step 时，二者均视为已知，相当于chicken-egg问题有了chicken。

具体的实现方式参考[Probability Linear Discriminant Analysis for Inference About Identy](https://wiki.inf.ed.ac.uk/twiki/pub/CSTR/ListenSemester2201112/prince-iccv07-plda.pdf) Appendix 中的公式28 以及MSR identity toolbox 中的glda_em.m实现



[An i-vector Extractor Suitable for Speaker Recognition with both Microphone and Telephone Speech](https://groups.csail.mit.edu/sls/publications/2010/Senoussaoui_Odyssey.pdf)
跨信道的处理方法



[A Study of Inter-Speaker Variability in Speaker Verification](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.494.6825&rep=rep1&type=pdf)

这个inter-speaker variability 和enginevoice 以及plda应该是一码事

[Joint Factor Analysis versus Eigenchannels in Speaker Recognition](https://www.crim.ca/perso/patrick.kenny/FASysJ.pdf)

这片文章的plda公式是 <a href="https://www.codecogs.com/eqnedit.php?latex=s&space;=&space;m&space;&plus;&space;vy&space;&plus;&space;dz" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s&space;=&space;m&space;&plus;&space;vy&space;&plus;&space;dz" title="s = m + vy + dz" /></a>   
v 张成的是本征音enginevoice空间, y 又称为common factors(共同因子？),其通过v 会影响s 中的所有维度，而z 被称为specific factors, 因为d 是对角阵diagonal matrix， z 中的每个维度只能影响对应的s中的一个维度，不像y中的一个维度会影响s 中的所有维度

公式(5)和（6）很像，但是（5）中s 是speaker dependent, 描述的是一个人的所有句子， 其对channel effect不敏感（因为是一个人很多句子的叠加吗？），
（6）中M 是spekaer- and channel- dependent ， 描述的是一句话， 所以其sensitive to channel effect


[A small footprint i-Vector Extractor](https://www.crim.ca/perso/patrick.kenny/kenny_odyssey2012.pdf)
学了一个词，precision matrix, 指协方差矩阵。
既然假设latent variable 是满足高斯分布，因此分析latent variable 只需分析其均值和方差即可。
所以提取公式中： <a href="https://www.codecogs.com/eqnedit.php?latex=Cov(y,y)&space;=&space;(I&space;&plus;&space;\sum_{c}N_cV_c^*\Sigma_c^{-1}V_c&space;)^{-1}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Cov(y,y)&space;=&space;(I&space;&plus;&space;\sum_{c}N_cV_c^*\Sigma_c^{-1}V_c&space;)^{-1}" title="Cov(y,y) = (I + \sum_{c}N_cV_c^*\Sigma_c^{-1}V_c )^{-1}" /></a>
是labtent variable y 的方差：
<a href="https://www.codecogs.com/eqnedit.php?latex=\langle&space;y&space;\rangle&space;=&space;Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_c)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\langle&space;y&space;\rangle&space;=&space;Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_c)" title="\langle y \rangle = Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_c)" /></a>

求latent variable y的时候先对x进行均值方差归一化
<a href="https://www.codecogs.com/eqnedit.php?latex=\Sigma_c^{-1}(F_c-N_cm_c)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Sigma_c^{-1}(F_c-N_cm_c)" title="\Sigma_c^{-1}(F_c-N_cm_c)" /></a>
再乘上转化矩阵的inverse从而进入y空间，
<a href="https://www.codecogs.com/eqnedit.php?latex=V_c^*\Sigma_c^{-1}(F_c-N_cm_c)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?V_c^*\Sigma_c^{-1}(F_c-N_cm_c)" title="V_c^*\Sigma_c^{-1}(F_c-N_cm_c)" /></a>
然后再y空间内方差归一化：
<a href="https://www.codecogs.com/eqnedit.php?latex=Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_)" title="Cov(y,y)\sum_cV_c^*\Sigma_c^{-1}(F_c-N_cm_)" /></a>


[A Straightforward and Efficient IMplementation of the Factor Analysis Model for Speaker Verification](https://alize.univ-avignon.fr/doc/publis/07_Interspeech_Matrouf.pdf)
plda 的训练过程，[MSR identity toolbox](https://www.microsoft.com/en-us/research/wp-content/uploads/2013/09/MSR-Identity-Toolbox-v1_1.pdf) 疑似按照这片文章写的代码
 
详见其中的train_tv_space.m文件
[Analysis of I-vector Length Normalization in Speaker Recognition System](https://pdfs.semanticscholar.org/1323/72db185b502e58765104c1465985fcab053a.pdf)
介绍了plda score 的计算方式
在这篇文章中指出，plda也是只估计一个矩阵，而不是标准公式中speaker 一个矩阵，channel 一个矩阵

这个训练的过程参照[Probability Linear Discriminant Analysis for Inference About Identy](https://wiki.inf.ed.ac.uk/twiki/pub/CSTR/ListenSemester2201112/prince-iccv07-plda.pdf)的公式11，
以及Appendix 1: Learning PLDA Models

[参看博客PLDA原理和em训练](https://blog.csdn.net/robingao1994/article/details/81905558)

E step: 求latent variable 的均值和方差  
M step: 最大化参数，即转化矩阵


[Bayesian Speaker Verification with Heavy-Tailed Priors](https://www.crim.ca/perso/patrick.kenny/kenny_Odyssey2010.pdf)

[人脸识别经典算法一：特征脸算法](https://blog.csdn.net/smartempire/article/details/21406005)

总结：训练过程需要求出：  
        1. mean face  
        2. eigenfaces

求eigenfaces时需要求解<a href="https://www.codecogs.com/eqnedit.php?latex=\Phi\Phi^T" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Phi\Phi^T" title="\Phi\Phi^T" /></a> 的特征值， 若图片有n张，每张像素为m, 则这个<a href="https://www.codecogs.com/eqnedit.php?latex=\Phi\Phi^T" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Phi\Phi^T" title="\Phi\Phi^T" /></a> 为 (n*m)(m*n)

因为<a href="https://www.codecogs.com/eqnedit.php?latex=\Phi\Phi^T" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Phi\Phi^T" title="\Phi\Phi^T" /></a> 

与

<a href="https://www.codecogs.com/eqnedit.php?latex=\Phi^T\Phi" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Phi^T\Phi" title="\Phi^T\Phi" /></a> 
具有相同的特征向量与特征知，所以如果m < n的话，可以计算（m*n）(n*m)的特征向量，而不是计算高纬度的（n*m）(n*m)的特征向量

求解出的这些特征向量统称为enginefaces, 
待测图片的模型为<a href="https://www.codecogs.com/eqnedit.php?latex=\omega_k&space;=&space;\mu^T_k(\Gamma&space;-&space;\Psi)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\omega_k&space;=&space;\mu^T_k(\Gamma&space;-&space;\Psi)" title="\omega_k = \mu^T_k(\Gamma - \Psi)" /></a>
 然后与注册的模版脸进行距离匹配 <a href="https://www.codecogs.com/eqnedit.php?latex=\varepsilon_k=\|&space;\Omega&space;-&space;\Omega_k\|^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\varepsilon_k=\|&space;\Omega&space;-&space;\Omega_k\|^2" title="\varepsilon_k=\| \Omega - \Omega_k\|^2" /></a>
 
 求图片中脸的模型<a href="https://www.codecogs.com/eqnedit.php?latex=\omega" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\omega" title="\omega" /></a> 计算的过程可以看作两步：
 1. 均值归一化，
 2。 与特征脸engineface （特征向量）求内积，即在该向量方向上的投影，因此可把enginefaces 看成一组bases, 最终求的脸的表示即在该bases下的坐标， 即特征脸代表了截然不同（正交的）的脸的维度，每一张脸都可以用各包含多少份（坐标，内积，投影）这样的特征脸来表示
 
 