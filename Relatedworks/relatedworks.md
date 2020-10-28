# Related works

1. Bond, F., Amati, F., & Blousson, G. (2015). Blockchain, academic verification use case. Technical Report). Retrieved from https://s3.amazonaws.com/signaturausercontent/blockchain_academic_verification_use_case.pdf.
2. Xu, Y., Zhao, S., Kong, L., Zheng, Y., Zhang, S., & Li, Q. (2017, October). ECBC: A high performance educational certificate blockchain with efficient query. In International Colloquium on Theoretical Aspects of Computing (pp. 288-304). Springer, Cham.
3. MIT Media Lab. (2017, November). Digital Certificates Project  [Scholarly project]. Retrieved from http://certificates.media.mit.edu/#home
4. Cheng, J. C., Lee, N. Y., Chi, C., & Chen, Y. H. (2018, April). Blockchain and smart contract for digital certificate. In *2018 IEEE international conference on applied system invention (ICASI)* (pp. 1046-1051). IEEE.







The exploration of digital academic certification system can be traced back to 2015. Federico Bond, Franco Amat and Gonzalo Blousson(2015) proposed a solution to simplify the verification of academic certificates and prevent academic certificate fraud. The solution is implementing the digital signature scheme and timestamps used in blockchain technology~\cite{Bond}. Federico, Franco and Gonzalo explain that the authenticity, integrity and non-repudiation of digital academic certificates are ensured by the digital signature scheme, and the internal fraud is prevented by timestamp. Yuqin Xu, Shangli Zhao, Lanju Kong, Yongqing Zheng and Shidong Zhang(2017) presents educational certificate blockchain(ECBC). ECBC uses MPT tree to speed up the inquiry for transactions and takes advantage of the cooperation of peers to reduce the latency~\cite{Xu}. The structure of ECBC is complete, but there is not a standard form of certification.

The first blockchain certification standard is created by The Massachusetts Institute of Technology and Central New Mexico Community College(2017). They trying to support the digital academic certificates through an application based on blockchain. They publish an open-source digital academic certification standard called Blockcerts~\cite{MIT}. The publication of Blockcerts define the standard of blockchain credentials format, and the Blockcerts is widely recognized by universities. Base on the Blockcerts, Jiin-Chiou Cheng, Narn-Yih Lee, Chien Chi, and Yi-Hua Chen(2018) designed a digital certificate system based on Ethereum blockchain. They use the smart contract of Ethereum to expand the scope of application, and support programming language Solidity to improve the efficiency function at each stage~\cite{Cheng}. 

There is still an unsolved problem about blockchain certification system, the reliability of certification authority. How can we trust a certification authority? In the past, people trust a certification because of he know and trust the authority. However, it is impossible for a verifier to recognize all the authority in the world. So, the problem is how to judge the reliability of an authority. The solution we proposed  is the Credit Rating System(CRS). Each time the verifier acknowledge a certification, the credit rating of its certification authority will increase. Which means the more verifier acknowledge an certification authority, the credit rating of the authority is higher and the authority is more reliable. The verifier can judge the reliability of a certification by checking the credit rating of its authority.

