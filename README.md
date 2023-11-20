import numpy as np
import matplotlib.pyplot as plt

# Defining our problem
a = 110
lengthx = 50 #milimeters
lengthy = 60
time = 3600 #time interval
nodes_x = 50
nodes_y= 60

# Initialization
dx = lengthx / nodes_x
dy = lengthy / nodes_y
dt = min(dx**2 / (4 * a), dy**2 / (4 * a))
t_nodes = int(time/dt)



u = np.zeros((nodes_x, nodes_y)) + 10 # Plate is initially at 10 degrees C

# Boundary Conditions
u[lengthx-1, 10:30] = 25
u[25:45,lengthy-1] = 24
u[25:45,20] = 24
u[0,10:30]=24

# Simulating
counter = 0
for k in range(time):
    for i in range(1, nodes_x - 1):
        for j in range(1, nodes_y -1):
            dd_ux = (u[i-1, j] - 2*u[i, j] + u[i+1, j])/dx**2
            dd_uy = (u[i, j-1] - 2*u[i, j] + u[i, j+1])/dy**2
            u[i, j] = dt * a * (dd_ux + dd_uy) + u[i, j]

# Visualizing with a plot
fig, axis = plt.subplots(figsize=(10,6))
pcm = axis.pcolormesh(u, cmap=plt.cm.jet, vmin=0, vmax=24)
plt.colorbar(pcm, ax=axis)
plt.savefig("final_heat_distribution2.png")
plt.show()
