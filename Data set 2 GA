import random
import csv

N = 80 #size of gene
P = 1000 #size of population
G = 150 #number of generations

class individual:
    gene = None
    fitness = None

    def __init__(self, N):
        self.gene = []
        self.fitness = 0
        j = 0
        for i in range(N):
            # generate random binary integer, including wildcard for conditions
            numCondition = random.randint(0,2)
            numAction = random.randint(0,1)
            # append each digit to gene
            if(i == j*8+7):
                self.gene.append(numAction)
                j += 1
            else:
                self.gene.append(numCondition)

#Do I need this class?
class offspr:
    gene = None
    fitness = None

    def __init__(self):
        self.gene = []
        self.fitness = 0

class rule:
    condition = None
    action = None

    def __init__(self):
        self.condition = []
        self.action = 0

population = []

for i in range(P):
    # for each method in population, append new individual
    population.append(individual(N))

def createRules(population, dataList):
    for i in population:
        rules = []
        for j in range(0, int(N/8)):
            #create instance of rule class
            r = rule()
            #declare rules and add to class
            for l in range(0,7):
                r.condition.append(i.gene[j*8+l])
            r.action = i.gene[j*8+7]
            #append to rules array
            rules.append(r)
        match = 0
        for k in dataList:
            for l in rules:
                for m in range(0, 7):
                    #print(int(l[m]), k.condition[m])
                    #if match
                    if int(k[m]) == l.condition[m]:
                        #increment match
                        match += 1
                    #if wildcard found
                    elif l.condition[m] == 2:
                        #increment match
                        match += 1
                #if all 7 integers match
                if(match == 7):
                    #if action match
                    if int(k[7]) == l.action:
                        #increment fitness of solution in population
                        i.fitness += 1
                    break
                match = 0

def selection(population, offspring):
    for i in range(P):
        #select two random variables for offspring
        parent1 = random.randint(0, P - 1)
        parent2 = random.randint(0, P - 1)
        #compare fitness in
        if population[parent1].fitness >= population[parent2].fitness:
            offspring.append(population[parent1])
        else:
            offspring.append(population[parent2])
    #print(len(offspring))

def crossover(parent1, parent2, crossoverOffspring):
    #generate random crossover point
    crossoverPoint = random.randint(0, N)
    child = []
    child2 = []
    for i in range(N):
        #if not past crossover point, add parent1 integers to gene, else add parent2 integers
        if i <= crossoverPoint:
            child.append(parent1.gene[i])
            child2.append(parent2.gene[i])
        else:
            child.append(parent2.gene[i])
            child2.append(parent1.gene[i])
    #Add to offspring once finished
    o = offspr()
    o2 = offspr()
    o.gene = child
    o2.gene = child2
    crossoverOffspring.append(o)
    crossoverOffspring.append(o2)

def mutation(parent, mutatedOffspring):
    mrate = 0.08
    child = []
    for i in parent:
        #generate random decimal
        r = float(random.randrange(0, 1000)) / 1000
        #if random decimal is less than or equal to mutation rate
        if r <= mrate:
            #flip values
            if i == 0:
                r = random.randint(1,2)
                child.append(r)
            elif i == 1:
                r = 0
                s = 2
                t = random.randrange(0, 10)
                if t < 0.5:
                    child.append(r)
                else:
                    child.append(s)
            else:
                r = random.randint(0,1)
                child.append(r)
        else:
            child.append(i)
    o = offspr()
    o.gene = child
    mutatedOffspring.append(o)

dataList = []

with open('data2.txt') as f:
    result = [list(line.strip().replace(' ', '')) for line in f]

for i in result:
    dataList.append(i)

dataList = dataList[1:]

highestfitness = []

averagefitness = []

for i in range(G):
    #print offspring of current population BEFORE crossover and mutation
    createRules(population, dataList)# CORRECT
    offspring = []
    crossoverOffspring = []
    mutatedOffspring = []
    selection(population, offspring) #CORRECT
    for j in range(0, int(P/2)):
        crossover(offspring[j*2], offspring[j*2+1], crossoverOffspring) #CORRECT
    for j in crossoverOffspring:
        mutation(j.gene, mutatedOffspring)
    f = 0
    f1 = 64
    worstindex = 0
    o1 = offspr()
    tfitness = 0
    for j in range(P):
        tfitness += population[j].fitness
        if population[j].fitness > f:
            f = population[j].fitness
            o1.gene = population[j].gene
            o1.fitness = population[j].fitness
    afitness = tfitness/P
    highestfitness.append(f)
    averagefitness.append(afitness)
    #print("")
    for j in range(P):
        mutatedOffspring[j].fitness = 0
        population[j] = mutatedOffspring[j]
    for j in range(P):
        if population[j].fitness < f1:
            worstindex = j
    for j in range(P):
        if j == worstindex:
            population[worstindex] = o1
            population[worstindex].fitness = 0

print("population size = ", P)
print("number of generations = ", G)

for i in highestfitness:
    print(i)
for i in averagefitness:
    print(i)
