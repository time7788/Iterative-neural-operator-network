# Iterative-neural-operator-network
## 1.	Research object
Flat-plate film cooling
![image](https://user-images.githubusercontent.com/111938900/198201956-7546a67f-b60d-430f-87f5-f186882cbfdc.png)
## 2.	The iterative neural operator network (INO)
The film cooling problem is essentially a convective heat transfer problem, which can be comprehensively described by a set of convection diffusion partial differential equations (PDE) and their boundary conditions. The physical quantities solved by these equations generally follow the laws determined by the following PDE system:

![image](https://user-images.githubusercontent.com/111938900/198202058-3835a8a9-1666-4a09-b28a-65146b55214b.png)

The difference form can be expressed as:

![image](https://user-images.githubusercontent.com/111938900/198202093-9ffd8b50-4060-46c7-bb7d-3913d11f2d3f.png)

u is the physical quantity of the required solution, f is the nonlinear function about u and its derivatives, and Ω represents the calculation domain, Γ represents the boundary.

Based on the idea of convolutional neural network and PDE-net, we compared the feedforward process of neural network with the explicit solution of CFD, and proposed an iterative operator neural network driven by physical law. Considering that the differential operator is invariant in space and time in the physical law, the convolution operation and differential operation can be equivalent, so that the differential equation can be deduced automatically from the data. The INO was based on this idea, using convolution to approximate the differential operator and the space advancement, and loops to approximate time advancement.

![image](https://user-images.githubusercontent.com/111938900/198202120-e832d9f4-5761-459d-a5aa-031d1a454d7d.png)

t means the number of iterations, s means boundary condition.

In most cases, the data obtained from the measurements only come from surfaces, interfaces or cross-sections of the computational domain, so using such two-dimensional data to characterize three-dimensional problems will inevitably lead to non-conservation of mass or energy. In addition, the data obtained by the experiment usually do not contain all the physical quantities in the differential equations, which makes the differential equation system not closed. In order to solve the above problems, this model introduces the idea of hidden state (hs), and approximates the physical variables through several hidden state layers. In the network, hidden state participates in the iteration along with the supervised physical quantity, so that the problem satisfies the conservation law and the equation closure condition. Then, the converged steady-state result can be approximated as:

![image](https://user-images.githubusercontent.com/111938900/198202162-8906710c-d79f-4c1a-8406-5b24ef7fcaa1.png)
## 3.	Architecture of iterative operator neural network
![image](https://user-images.githubusercontent.com/111938900/198202446-dd504966-8dbc-4bef-83ad-a261e8c1e2f5.png)

The architecture of iterative operator neural network based on the above idea is shown in Figure. The input of the network includes the target physical quantity layer (u), a set of hidden physical quantity layers (hs) and the boundary condition layer (s) marking the boundary of the calculation domain. The boundary layer includes geometric layer, inlet boundary layer, outlet boundary layer and blowing ratio layer. The geometric layer is a matrix representing the hole distribution information. In the datasets selected in this study, the mainstream flow direction is from left to right. Therefore, the inlet layer is characterized by a matrix in which the value in the leftmost column is 1 and the rest are 0, the outlet layer is characterized by a matrix in which the value in the rightmost column is 1 and the rest are 0. The blowing ratio layer is characterized by multiplying the normalized blowing ratio value with the geometry layer. Besides, the target physical quantity layer and hidden physical quantity layer in the input layer of each iteration are the output of the previous iteration, while the boundary condition layer of each iteration remains unchanged.
## 4.	Loss function
![image](https://user-images.githubusercontent.com/111938900/198202464-0f8eaee6-bf70-4a76-8520-b26c02d1e181.png)

The first-order norm of the absolute error between the predicted image and the real image is used to measure the prediction accuracy of the model. The loss function of the network is shown in Figure, which is the sum of L1 losses in all iterative steps. With this setting, the model will converge to the real image as soon as possible.
## 5.	Dataset
![1](https://user-images.githubusercontent.com/111938900/198203392-4c85c62b-4bc9-4cba-aac9-12d928324464.png)

The dataset used for training network was obtained from CFD calculation results of flat-plate film cooling under different blowing ratio and hole distributions. Each CFD data was processed into two matrices. The input matrix had a dimension of 128×512×2, where the two image layers respectively represented the geometry of the jet plate and the blowing ratio.
## 6.	Requirement
$ pip install -r requirements.txt



