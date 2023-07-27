# Cas-protein-sliding-distance-simulation
import random
import numpy as np
import time

t1 = 0.1  # 3D searcning time (s)
t2 = 0.16  # PAM reading time (s)
V = 100  # sliding rate（bp/s）
X = 2000000  # DNA length（bp）
N = 125000  # PAM numbers
D1 = 16  # PAM interval

num_trials = 10000  # simulation time

# D2 iteration
D2_values = np.arange(1, 101, 1)
min_avg_flights = float("inf")  # minimum searching time
best_D2 = None  # the best D2

for D2 in D2_values:

    
    total_flights = 0  # total searching time

    for i in range(num_trials):
        bird_pos = random.uniform(0, X)  # random position
        num_flights = 0  # current searching time
        direction = random.choice([-1, 1])  # direction

        while True:
            bird_pos += direction * D2

            # PAM reading
            if int(bird_pos / D1) % 2 == 1:
                num_flights += 1
                time.sleep(t2)

            # find the target site
            if bird_pos < 0 or bird_pos > X:
                total_flights += num_flights
                break

            # sliding direction
            if random.random() < 0.5:
                direction = -direction

            # 3D collision time
            time.sleep(t1)
            num_flights += 1

    # average searching time
    avg_flights = total_flights / num_trials

    # the best D2
    if avg_flights < min_avg_flights:
        min_avg_flights = avg_flights
        best_D2 = D2

print("the best D2：{}".format(best_D2))


