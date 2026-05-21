import numpy as np
import matplotlib.pyplot as mpl

S = np.zeros(1826) #Susceptible 0 to 1825
I = np.zeros(1826) #Infected 0 to 1825
R = np.zeros(1826) #Removed/Recovered 0 to 1825

S[0] = 9999999
I[0] = 1
R[0] = 0

N = 10000000
Time = np.arange(0, 1826) #Time (days)

user1 = float(input("Enter the value of the transmission rate: "))
user2 = float(input("Enter the value of avg no. of days for recovery: "))

beta = user1 #transmission rate
rr  = user2 #recovery rate, like if its 5, so on avg people recover in 5 days

for T in range(0, 1825):  #1825 cases filled
 wantedinfections = beta * I[T] * S[T]/N
 actualinfections = min(wantedinfections, S[T]) # what this does is if virus wants to infect 20 people
 # but only 5 are suceptible then it would infect only 5 hence the value of susceptibele would never go
 # in negative
 S[T+1] = S[T] - actualinfections
 I[T+1] = I[T] + actualinfections - I[T]/ rr
 R[T+1] = R[T] + I[T]/ rr

mpl.plot(Time, S, label = "Susceptible")
mpl.plot(Time, I, label = "Infected")
mpl.plot(Time, R, label = "Removed (recovered or passed)")

mpl.xlabel("Time")
mpl.ylabel("People")
mpl.legend()
mpl.show()
R0 = beta * rr
if R0 > 1:
 print(f"R0 > 1 and thus outbreak grows and its value is, {R0} .")
elif R0 == 1:
 print("R0 = 1 and thus outbreak stays stable.")
else:
 print("R0 < 0 and thus outbreak stays contained and soon dies down.")
