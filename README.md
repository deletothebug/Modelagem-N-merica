2) Uma placa triangular de metal maciço, de catetos 5m × 5m (considere que sua espessura é muito menor do que essas dimensões), faz
parte de um maquinário. Cada borda dessa placa é submetida a uma temperatura constante. Numa situação de equilíbrio termostático, querse saber como a temperatura T se distribuirá em função da posição (x,y) (que localiza um ponto da placa). Esse problema é modelado pela
equação de Laplace. Escreva a equação satisfeita pela temperatura T(x,y) em cada vértice (x,y) da malha, isto é, a equação satisfeita por t1 ,
t2 , t3 , t4 , t5 e t6 .

Tirar a média de temperatura dos pontos vizinhos (os pontos abaixo, cima, esquerda e direita);

Sistema de equações gerado: t1 - 0,25t2 = 20

t2 - 0,25t1 - 0,25t3 - 0,25t4 = 0
t3 - 0,25t2 - 0,25t5 = 20
t4 - 0,25t2 - 0,25t5 = 1,25
t5 - 0,25t4 - 0,25t3 - 0,25t6 = 1,25
t6 - 0,25t5 = 21,25

A = np.array([[1, -0.25, 0, 0, 0, 0],[-0.25, 1, -0.25, -0.25, 0, 0], [0, -0.25, 1, 0, -0.25, 0], [0, -0.25, 0, 1, -0.25, 0], [0, 0, -0.25, -0.25, 1, -0.25], [0, 0, 0, 0, -0.25, 1]], dtype = float)
B = np.array([[20],[0],[20],[1.25],[1.25],[21.25]], dtype = float)
epsilon = 0.00001
num_max_int = 100000

n = A.shape[1]

betas = np.zeros(n, dtype=float)
for i in range(0,n):
betas[i] = (sum(abs(A[i,j])*betas[j] for j in range(0,i))+ sum(abs(A[i,j]) for j in range(i+1,n)))/abs(A[i,i])
beta = max(betas)
if beta < 1:
  resposta_cs = "é"
else:
  resposta_cs = "não é"
print("O valor de beta é {}, e portanto o critério de Sassenfeld {} satisfeito.\n".format(beta, resposta_cs))

X = np.zeros(n, dtype=float)
Var = 1
k = 0
while (Var > epsilon and k < num_max_int):
  k = k+1
  X_ant = np.copy(X)
  v= []
  for i in range (0, n):
s= sum (A[i,j]*X[j] for j in range(0,i))+ sum(A[i,j]*X_ant[j] for j in range(i+1, n))
X[i] = (B[i,0]-s)/A[i,i]
if X[i]!=0:
  v.append(abs(X[i]-X_ant[i]/X[i]))
elif (X[i]==0 and X_ant[i]==0):
  v.append(0)
elif (X[i] == 0 and X_ant[i]!=0):
  v.append(1)
Var = max(v)

print("Considerando uma precisão de {} e um máximo de {}, sendo a solução uma matriz igual a {} com {} iterações".format(epsilon, num_max_int, X, k))
O valor de beta é 0.5625, e portanto o critério de Sassenfeld é satisfeito.
