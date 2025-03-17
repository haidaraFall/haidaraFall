- 👋 Hi, I’m @haidaraFall
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
haidaraFall/haidaraFall is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import numpy as np
from scipy.integrate import solve_ivp
import matplotlib.pyplot as plt

# Paramètres du système
m1, m2, m4 = 0.2, 0.2, 0.2  # masses en kg
r1, r2, r3, r4, r5 = 0.057295779, 0.0364756, 0.057295779, 0.02322, 0.057295779  # rayons en m
I1, I2, I4 = m1 * r1**2, m2 * (r2**2 + r3**2) / 2, m4 * (r4**2 + r5**2) / 2  # moments d'inertie
g = 9.81  # accélération due à la gravité en m/s²

# Fonctions pour les termes des leviers et les dérivées des hauteurs
def f1(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4):
    # Exemple : termes des leviers et dérivées des hauteurs
    return m1 * g * np.sin(theta1)  # Simplification pour l'exemple

def f2(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4):
    # Exemple : termes des leviers et dérivées des hauteurs
    return m2 * g * np.sin(theta2)  # Simplification pour l'exemple

def f4(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4):
    # Exemple : termes des leviers et dérivées des hauteurs
    return m4 * g * np.sin(theta4)  # Simplification pour l'exemple

# Système d'EDO
def system(t, y):
    theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4 = y
    ddot_theta1 = -f1(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4) / I1
    ddot_theta2 = -f2(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4) / I2
    ddot_theta4 = -f4(theta1, theta2, theta4, dot_theta1, dot_theta2, dot_theta4) / I4
    return [dot_theta1, dot_theta2, dot_theta4, ddot_theta1, ddot_theta2, ddot_theta4]

# Conditions initiales
theta1_0 = 0.5  # Angle initial de la roue r1 en radians
theta2_0 = 0.0  # Angle initial du double balancier r2/r3 en radians
theta4_0 = 0.0  # Angle initial du double balancier r4/r5 en radians
dot_theta1_0 = 0.0  # Vitesse angulaire initiale de la roue r1 en rad/s
dot_theta2_0 = 0.0  # Vitesse angulaire initiale du double balancier r2/r3 en rad/s
dot_theta4_0 = 0.0  # Vitesse angulaire initiale du double balancier r4/r5 en rad/s
y0 = [theta1_0, theta2_0, theta4_0, dot_theta1_0, dot_theta2_0, dot_theta4_0]

# Intervalle de temps
t_span = (0, 10)  # De 0 à 10 secondes
t_eval = np.linspace(0, 10, 1000)  # Points de temps pour la solution

# Résolution du système d'EDO
sol = solve_ivp(system, t_span, y0, t_eval=t_eval)

# Trajectoires des angles
theta1_t = sol.y[0]
theta2_t = sol.y[1]
theta4_t = sol.y[2]

# Affichage des résultats
plt.figure(figsize=(10, 6))
plt.plot(sol.t, theta1_t, label=r'$\theta_1(t)$')
plt.plot(sol.t, theta2_t, label=r'$\theta_2(t)$')
plt.plot(sol.t, theta4_t, label=r'$\theta_4(t)$')
plt.xlabel('Temps (s)')
plt.ylabel('Angle (rad)')
plt.legend()
plt.grid()
plt.title('Trajectoires des angles en fonction du temps')
plt.show()
