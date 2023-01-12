# cia1-daa
"""
Created on Thu Jan 12 11:59:49 2023

@author: ai_ds-b1
"""


import numpy as np
MAX_SCORE=13
f=['A','T','C','G']
s=['T','A','C','T']
#for populating the matrix
a= np.zeros([5,5], dtype = int) 
print("INITIAL MATRIX=")
print(a)


class Scoree:
	def __init__(self,gap,match,mismatch):
		self.gap = gap
		self.match = match
		self.mismatch = mismatch

def Matrix(sizeX,sizeY):
	matrix = []
	for i in range(len(sizeY)+1):
		subMatrix = []
		for j in range(len(sizeX)+1):
			subMatrix.append(0)
		matrix.append(subMatrix)
	return matrix

def ALLIGN(x,y,score):
	matrix = Matrix(x,y)
	best = 0
	o= (0,0)

	for i in range(1,len(y)+1):
		for j in range(1,len(x)+1):
			matrix[i][j] = max(
				matrix[i][j-1] + score.gap,
				matrix[i-1][j] + score.gap,
				matrix[i-1][j-1] + (score.match if x[j-1] == y[i-1] else score.mismatch),
				0
				)

			if matrix[i][j] >= best:
				best = matrix[i][j]
				o = (i,j)

	return best, o, matrix

def printMatrix(matrix):
	for i in range(len(matrix)):
		print(matrix[i])
	print()

def getSequence(x,best,o,matrix):
	seq = ''
	i = o[0]
	j = o[1]

	while(i > 0 or j > 0):

		diag = matrix[i-1][j-1]
		up = matrix[i-1][j]
		left = matrix[i][j-1]

		if min(diag,left,up) == diag:
			break
		else:
			i = i - 1
			j = j - 1
			seq += x[j]
	return seq[::-1]
x = 'ATCG'
y = 'TACT'
print('Input: ')
print(x)
print(y)
score = Scoree(-1,8,-3)
best, o, matrix = ALLIGN(x,y,score)

print('allignment matrix:')
printMatrix(matrix)

print('Highest number in matrix:' +str(best))
