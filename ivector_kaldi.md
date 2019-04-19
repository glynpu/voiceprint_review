Dehak [Front-end factor analysis for speaker verification]()

A review of  ["I-vector Extraction for Speaker Recognition Based on Dimensionality Reduction"](https://pdf.sciencedirectassets.com/280203/1-s2.0-S1877050918X00118/1-s2.0-S1877050918314042/main.pdf?x-amz-security-token=AgoJb3JpZ2luX2VjEHoaCXVzLWVhc3QtMSJGMEQCIDIEP4zGiqfC0hbEv7pZCx9edyUc0oe0jlzJVlnMGiOfAiBRKrfBAznANc45m85IW5CQ%2BeJ3NdKIyG8v%2FmUJwtkXXyraAwhSEAIaDDA1OTAwMzU0Njg2NSIMpUe1wwSVD9oIBk5fKrcD9gZTuQLraKsF%2Fc8EE87OEMn1dSpVFItkJDQTXlNYbkqXYNfflF%2F1dLGn1r1R8s7J0Ixkz88DVT2lj0S3FpypSbOpSbT0xGv8mVCYPjapvTmIHubJqE6JEym06qNJpFaopyePwR0tycIYy09Szfxq6fLbWTy229Rcifaqj%2FAPKXWxUozf010FWsZKuAORZHkrdCqJe3HpLpd2ULzIC2%2FxuXVBr%2BTF%2BqIzHoiea9E6ycZozeaikOfO%2B3OAOPjGirJVt%2FKzWbhwEDJhdcGXbOalL0Y8aeI6XjEcEgz4VzRery6cuKWLqoF047E0cSdP6MnLbYQNiGdIm%2FWUqWwQZOvCvnjcduYaW5NnRehNmF9ELMw14hyC6AS74HpCH3YwkKfj%2Ft2fW5ZKPLFTy077gNB4qCmQD4Al8K0KisVk4hjNJSw0WdaxOc9nmdpU5BCxuWG%2BTHHK4fC5m7HNxwCyFxTzQyH05DVixIbraExN5oz69W1qnxOA%2BBpDXWB44b6opwyrAg7weg1E6loAPq8AUcqpfw1DC2va7mBxCZYC9LuLv%2BRB%2F0iBfl8q%2BO%2FlidugIjZweU0416gPNTDixOTlBTq1AbGMZSqNnClInctK%2B6cNm%2BHCJS1rk7qvyjph384ZbFXD6dEyM6E2KyDEIoUDtuVq9WLquQ2uTVpqHkFV5H0CdbOxuzdaOoD7dLaWTlX5e5yD2bFSOKO%2FKoVUd5rZw5HUH9exGeQ201MTIS2coMMyxJGnIBKPh8YponVJoyLeuhF%2FMaf7zQ7C4gwXg8xLtG3bHnaefzfH4njiB5oKDWvpEuS74mSO6Fct6NngyE6UKlHv4pGGAso%3D&AWSAccessKeyId=ASIAQ3PHCVTYRYBOUGVA&Expires=1555640659&Signature=saSNSF%2BiTuuyV%2BtWKM1dkletLfU%3D&hash=7263ac8e3a142c2f3e178c3a11a71cc2e902561f244b6548c4946c506875f3ef&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S1877050918314042&tid=spdf-118b68a7-1f5a-4a4a-a34f-44b11b358c1b&sid=50d782432bc9b345795a46a950b23b7f8603gxrqb&type=client)
published in 2018

Abstract of the abstract:
what is 2656 syllables bio-acoustic signals from 55 species of frog
1. i-vector is a factor analysis and a subspace modeling method
2. Total Variability(TV): a low dimensional speaker- and channel-dependent space is defined
3. Total means both speaker and channel variabilities
4. Benefit(?) of modelling both the intra-domain and inter-domain variabilities into the same low dimensional space
5. Smaller UBM and higher i-vector is better


Abstract of Introduction
1. Terms: channel variability == session variability 
2. Channel compensation and noise reduction gives improvements over TV
3. i-vector(total variability model) has many data-driven components 
4. hyper-parameters can be trained on a completely different out-of-set-data
5. traditional appraoch is GMM 
6. Unlike the separate speaker and channel dependent subspace of JFA, i-vectors represent the GMM super-vector by a single total variability space
7. channel space contains information that can be used ot distinguish between speakers

How:
1. i-vector is a space transformation method, i.e. a transformation matrix: The approach provides an elegant way of reducing high-dimensional sequential input data to a low-dimensional fixed-lenght feature vector

    <a href="https://www.codecogs.com/eqnedit.php?latex=s&space;=&space;m&space;&plus;&space;Tw" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s&space;=&space;m&space;&plus;&space;Tw" title="s = m + Tw" /></a>  

    m is mean supervector  
    T is a matrix bases spanning the subspace covering the important variability  
    w is i-vector which is a standard normally distributed latent variable, and the Maximum A posterior(MAP) point estimate(极大后验点估计) of the latent variable w
    i-vector dimensionality is equal to the "rank" of eigenmatrix, a larger i-vector dimension would not give much improvement to the classification performance but it greatly increased the computation effort
    
  
Process
1. front-end: cepstral feature extraction and UBM training
2. back-end: sufficient statistics computation, T-matrix training, i-vector extraction, dimensionality reduction and scoring  
![](https://github.com/glynpu/voiceprint_review/blob/master/jpeg/block_diagram_for_frog_identification_system_experiment.png)



 

<a href="https://www.codecogs.com/eqnedit.php?latex=ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" title="ax^{2} + by^{2} + c = 0" /></a>