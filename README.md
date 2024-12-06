text = """"""

class Node:
    def __init__(self, value, child):
        self.element = value
        self.parents = []
        self.children = []
        if child is not None:
            self.children.append(child)

    def getChildren(self):
        return self.children

    def getParents(self):
        return self.parents
    
    def getElement(self):
        return self.element
        
    def updateChildren(self, childvalue):
        self.children.append(childvalue)
        return self.children

    def updateParent(self, parentvalue):
        self.parents.append(parentvalue)
        return self.parents
    
    def toString(self):
        return "Value: " + str(self.element) + " Parents: " + str(self.parents) + " Children: " + str(self.children) + "\n"

class Graph:
    def __init__(self):
        self.graph = {}
    
    def addNode(self, value, child):
        if value in self.graph.keys():
            self.graph[value].updateChildren(child)
        else:
            self.graph[value] = Node(value, child)
        if child in self.graph.keys():
            self.graph[child].updateParent(value)
        else:
            self.graph[child] = Node(child, None)
    
    def isValid(self, case):
        left = case[0]
        for idx in range(1, len(case)):
            right = case[idx]
            if(left in self.graph[right].getChildren()):
                return False
            if(right not in self.graph[left].getChildren()):
                return False
            left = right
        return True
        
    def correctCase(self, case):
        keepgoing = True
        while keepgoing == True:
            keepgoing = False
            for idx in range(1,len(case)):
                if(case[idx-1] in self.graph[case[idx]].getChildren() or case[idx] not in self.graph[case[idx-1]].getChildren()):
                    tmp = case[idx]
                    case[idx] = case[idx-1]
                    case[idx-1] = tmp
                    keepgoing = True
        return case[int((len(case)-1)/2)]

    def toString(self):
        graph = ""
        for node in self.graph.values():
            graph += node.toString()
        return graph

input = text.split("\n\n")
nums_raw = input[0]
cases_raw = input[1]

graph = Graph()

for numstext in nums_raw.splitlines():
    nums = list(map(int,numstext.split("|")))
    graph.addNode(nums[0], nums[1])

validcases = []
invalidcases = []

for casestext in cases_raw.splitlines():
    case = list(map(int,casestext.split(",")))
    valid = graph.isValid(case)
    if valid:
        validcases.append(case)
    else:
        invalidcases.append(case)
        
sum = 0
for validcase in validcases:
    middle = (len(validcase)-1)/2
    sum += validcase[int(middle)]

print(str(sum))

keepgoing = False
sum = 0

import copy
for invalidcase in invalidcases:
    case = copy.deepcopy(invalidcase)
    sum += graph.correctCase(case)
    
print(str(sum))

































text = """....#.....
.........#
..........
..#.......
.......#..
..........
.#..^.....
........#.
#.........
......#..."""

carat_pos = [0,0]

lines = text.splitlines()

for idx in range(len(lines)):
    for jdx in range(len(lines[idx])):
        if(lines[idx][jdx] == "^"):
            carat_pos = [idx,jdx]

linesarr = [] 
for line in lines:
    linesarr.append(list(line))
lines = linesarr

def move(position, direction, lines):
    if(direction == "N"):
        if(position[0] - 1 < 0):
            lines[position[0]][position[1]] = "X"
            return
        newposition = [position[0] - 1, position[1]]
        newdirection = direction
        if(lines[newposition[0]][newposition[1]] == "#"):
            newposition = position # keep og position if we turn
            lines[newposition[0]][newposition[1]] = ">"
            newdirection = "E"
        else:
            lines[newposition[0]][newposition[1]] = "^"
            lines[position[0]][position[1]] = "X"
        move(newposition, newdirection, lines)
    elif(direction == "S"):
        if(position[0] + 1 >= len(lines)):
            lines[position[0]][position[1]] = "X"
            return
        newposition = [position[0] + 1, position[1]]
        newdirection = direction
        if(lines[newposition[0]][newposition[1]] == "#"):
            newposition = position # keep og position if we turn
            lines[newposition[0]][newposition[1]] = "<"
            newdirection = "W"
        else:
            lines[newposition[0]][newposition[1]] = "v"
            lines[position[0]][position[1]] = "X"
        move(newposition, newdirection, lines)
    elif(direction == "E"):
        if(position[1] + 1 >= len(lines[0])):
            lines[position[0]][position[1]] = "X"
            return
        newposition = [position[0], position[1] + 1]
        newdirection = direction
        if(lines[newposition[0]][newposition[1]] == "#"):
            newposition = position # keep og position if we turn
            lines[newposition[0]][newposition[1]] = "move"
            lines[newposition[0]][newposition[1]] = "v"
            newdirection = "S"
        else:
            lines[newposition[0]][newposition[1]] = ">"
            lines[position[0]][position[1]] = "X"
        move(newposition, newdirection, lines)
    elif(direction == "W"):
        if(position[1] - 1 < 0):
            lines[position[0]][position[1]] = "X"
            return
        newposition = [position[0], position[1] - 1]
        newdirection = direction
        if(lines[newposition[0]][newposition[1]] == "#"):
            newposition = position # keep og position if we turn
            lines[newposition[0]][newposition[1]] = "move"
            lines[newposition[0]][newposition[1]] = "^"
            newdirection = "N"
        else:
            lines[newposition[0]][newposition[1]] = "<"
            lines[position[0]][position[1]] = "X"
        move(newposition, newdirection, lines)

move(carat_pos, "N", lines)

counter = 0
for idx in range(len(lines)):
    for jdx in range(len(lines[idx])):
        if(lines[idx][jdx] == "X"):
            counter += 1
print(counter)
