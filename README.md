# exercise-6
***
###Abstract
This assignment is mainly formed by 2 parts:
L1 Adding Coriolis force and air resistence(**which I've done before**)
L2 Super Auxillary Specific Target System
***
###Background
>2.10 Generalize the program developed for the previous problem so that it can deal with situations in which the target is at a different altitude than the cannon.Consider cases in which the target is higher and lower than the cannon. Also investigate how the minimum firing velocity required to hit the target varies as the altitude of the target is varied.

####Level 1:Take the ari resistence of headwind into consideration
####Level 2:Consider the variance of launch angle, velocity and air resistance, designing an auxillary system to make the cannon shell hit the target more accurately.
***
###Exercise
####L1:Adding Coriolis force and air friction(considering the temperature):
#####I've done this step in my last assignment, here I'll only show some pictures,for more details, please click [here](https://github.com/LuxAsteria/exercise5)
####step 1: Considering about the temperature and the natural wind
![t_w](https://github.com/LuxAsteria/test3/blob/master/t_w%2019(wz)%20100(z).png)
####step 2: Taking Coriolis into consideration but exclude the influence of wind :
Consider the force in the following coordinates:
![coriolis](https://github.com/LuxAsteria/test3/blob/master/coriolis%20force.png)

Here's the screenshot:
![t_c](https://github.com/LuxAsteria/test3/blob/master/t_c_39(wz)100(z)%203d.png)
![t_c2d](https://github.com/LuxAsteria/test3/blob/master/t_c_139(wz)100(z).png)
####step 3:Considering Coriolis force, natural wind and the effect of temperature:

The screenshot of the simulation:

![pic](https://github.com/LuxAsteria/test3/blob/master/t_c_w_39(wz)100(z)_3d.png)
![pic2d](https://github.com/LuxAsteria/test3/blob/master/t_c_w%2039(wz)100(z).png)
####step4:Comparing the data:
First, get the maximum of height and distance:
```
def maxi(self):
        print(max(self.xz))
        print(max(self.xx))
```
The data are put in the table:

First, show the differeces caused by changes in angel beteewn velocity(wind velocity) and the z coordinate

![table](https://github.com/LuxAsteria/test3/blob/master/table.png)

Second, show the differences caused by the different angle between angular velocity and z coordinate:
![table](https://github.com/LuxAsteria/test3/blob/master/lati.png)

####setp 5:Found in pictures:
![pic](https://github.com/LuxAsteria/test3/blob/master/t_c_39(wz)120(z)3d.png)

It's apparent that the trajectory would tilt when we set the simulation in 3d projection and taking the following factors:Coriolis force, air firction,etc. into consideration.

####Level 2:Super Auxillary Specific Target System:
###Analyse:
The system is designed to help the cannon to aim the target.Here, I'm going to talking about the 2d situation.
Thus, the initial position, including x_target,y_target,v_wind,v_initial_shell or angle_initial_shell.What we need to check is the initial velocity and the original angle.
In order to do that, first,check the velocity.Here's the code:
```
class t:
    def find_x(self,angle=40,v=700,vwind=4):
        self.x=[0]
        self.y=[0]
        self.vx=[v*math.cos(angle)]
        self.vy=[v*math.sin(angle)]
        self.dt=0.02
        self.angle=angle
        self.apha=2.5
        self.b=4*pow(10,-5)
        self.a=6.5*pow(10,-3)/300
        self.g=9.8
        self.v=[v]
        while self.y[-1]>=0:
            vx=self.vx[-1]-self.dt*self.b*(1-self.a*self.y[-1])**self.apha*self.v[-1]*self.vx[-1]-self.b*self.dt*(1-self.a*self.y[-1])**slef.apha*self.vwind**2
            vy=self.vy[-1]-self.dt*self.b*(1-self.a*self.y[-1])**self.apha*self.v[-1]*self.vx[-1]-self.dt*self.g
            self.vx.append(vx)
            self.vy.append(vy)
            x=self.x[-1]+self.dt*self.vx[-1]
            y=self.y[-1]+self.dt*self.vy[-1]
            self.y.append(y)
            self.x.append(x)
            return abs(self.x[-1])
    def find_angle(self,x=12,angle=40,step=100):
        angles=list(np.linspace(0,np.pi/2,step))
        range_=[]
        for i in angles:
            r=t.find_x(self,angle=i,v=700,vwind=4)
            range_.append(r)
            max_range=max(range_)
            max_angle=angles[range_.index(max_range)]
            return(max_range,max_angle)
    def angl(self,accu=1,x=10):
        s=int(40000/accu)
        mr=t.find_angle(self,x=s,angle=40,step=100)[0]
        ma=t.find_angle(self,x=s,angle=40,step=100)[1]
        if x>mr:
            print(233)
        elif x==mr:
            return ma,mr
        else:
            angle_1=ma
            angle_2=mr
        while t.find_x(self,angle=angle_1,v=700,vwind=4)>(x+10*accu/2.0):
            angle_1 -= 0.0001
        while t.find_x(self,angle=angle_2,v=700,vwind=4)>(x+10*accu/2.0):
            angle_2 += 0.0001
        while t.find_x(self,angle=angle_1,v=700,vwind=4)>(x+accu/2.0):
            angle_1 -= 0.00001
        while t.find_x(self,angle=angle_2,v=700,vwind=4)>(x+accu/2.0):
            angle_2 += 0.00001
        if t.find_x(self,angle=angle_1,v=700,vwind=4)>=t.find_x(self,angle=angle_2,v=700,vwind=4):
            an=angle_2
            ra=t.find_x(self,angle=angle_2,v=700,vwind=4)
        else:
            an=angle_1
            ra=t.find_x(self,angle=angle_1,v=700,vwind=4)
            return (an,ra)
        print(an,ra)
k=t()
k.find_x()
k.find_angle()
k.angl()
```
Here, to express the uncertainty of the cannon, we introduce the fuction'normalvariate':
```
v=random.normalvariat(v,v*uncertainty)
angle=random.normalvariate(angle,angle*uncertainty)
vwind=random.normalvariate(angle,angle*uncertainty)
```
in which angle is required to be got from the position x of the target.
so we can test our programme using those codes.
Based on this code, we can also check the velocity:
```
def find_speed(self,x=10,angle=40,step=100):
        velos=list(np.linspace(200,1000,step))
        range_=[]
        for i in velos:
            angle=t.angl(self,accu=1,x=10)
            r=t.find_x(self.angle,v=i,vwind=4)
            range_.append(r)
            max_range=max(range_)
            max_velo=velos[range_.index(max_range)]
            return(max_range,max_velo)
    def velo(self,accu=2,x=10):
        angle_=t.angl(self,accu=1,x=10)
        s=int(400000/accu)
        mr=t.find_x(self,angle=angle_,v=700,vwind=4)[0]
        ma=t.find_x(self,angle=angle_,v=700,vwind=4)[1]
        if x>mr:
            print(233)
        elif x==mr:
            return ma,mr
        else:
            velo_1=ma
            velo_2=mr
            while t.findx_x(self,angle=angle_,v=velo_1,vwind=4,anglw=30)>(x + 10*accu/2.0):
                velo_1 -= 0.0001
            while t.find_x(self,angle=angle_,v=velo_2,vwind=4,anglw=30)>(x+10*accu/2.0):
                velo_2+=0.0001
            while t.find_x(self,angle=angle_,v=velo_1,vwind=4,anglw=30)>(x+accu/2):
                velo_1-=0.00001
            while t.find_x(self,angle=angle_,v=velo_2,vwind=4,anglw=30)>(x+accu/2):
                velo_2+=0.0001
        if t.find_x(self,v=velo_1,vwind=4,anglw=30,angle=angle_)>t.find_x(self,v=velo_2,angle=angle_,anglw=30,vwind=4):
            ve=velo_1
        else:
            ve=velo_2
        return(ve)
```
####This programm could still be improved(not ready for competing).
![imoge](https://github.com/LuxAsteria/test3/blob/051ab16c9262949e71ca8329e6595f48402f374b/u%3D3602176617%2C867144629%26fm%3D21%26gp%3D0.jpg)
***
###Conclusion
Here in this assignment,I've done 2 levels of the homework.Still, the auxillary programme could be improved to apply in 3d system.
***
###Acknowledge
[1]Senior WuYuqiao's [code](https://github.com/wuyuqiao/computationalphysics_N2013301020142)
[2]Python tutorial(python.org)


#Ps:All Rights Reserved!
