# Q_Wander
Wander Through Deep Q


## Whats The Point?  
While developing your Deep-Q system you will need to decide how you traverse the environment. Although
there are many ways to accomplish this, there are usually 3 main objectives we would like to impliment:
#### 1) Explore More Early (move randomly more as opposed to through a decision making process )
#### 2) Explore Less as time passes (move more through decision making as opposed to random chance through time)
#### 3) Always retain some level of random behavior even at the longest time periods to avoid bias and retain dynamic "problem solving" capabilities 
    
# Input Parameters  
`t` Time tracking variable   
`Eps` Minimum epsilon value  
`Max_Eps` Maximum epsilon value  
`Del_Eps` Defined as `Max_Eps - Eps` this will be the upper bound of your randomizing function   
`t_fac` The time when you want the system to be at maximum probability of decision based process  
`run_fac` Takes on the property of `t / t_fac` until you approach `t_fac` where it may take on the property of `Del_Eps`   

# Silly Example  

First we initialize our variables  
We can choose our epsilon values to be anywhere from 0 to 1 but to reduce edge effects we choose .1 and .9  
We also choose a `t_fac` of 100 for no real reason here, but in the real world you would base this on the problem you where solving and the size of your deep-Q memory.  

```R
t=0
Eps=.1
Max_Eps=.9
Del_Eps= Max_Eps-Eps
t_fac=100
```  

We want to see the behavior of this system so we will need some vectors to store our data as we itterate through time  
```R
BEHAVE=c()
SM=c()
```  
We will run the simulation 200 time units to see the behavior that arrises from our choices in variables
```R
for (i in 1:200)
{
 
if ((t/t_fac) > (Del_Eps)) 
	{run_fac=(Del_Eps)}
	else{run_fac=(t/t_fac)}

smpl=sample(seq(0,1,length=100),1,replace=FALSE)

if (smpl < (Eps+run_fac))
	{BEHAVE[i]=1}
	else{BEHAVE[i]=0}

t=t+1
SM[i]=sum(BEHAVE)
}
```  
Now we can plot the results  
```R
cols=rep("cyan",length(SM))
cols[which(BEHAVE=="1")]<-"darkred"
dev.new()
par(bg="grey")
par(mfrow=c(2,1)) 
plot(BEHAVE,type="l",main="Random Vs Non Random By Time",xlab="Time") 
points(BEHAVE,pch=16,col=cols) 

plot(SM,type="l", main="Cumulative Non Random Decisions By Time",xlab="Time")
points(SM,pch=16,col=cols)
```  
