# DC

def flatten(text):
    cats = text.splitlines() #untrimmed categories
    rootNode = "0.  "
    categories = [rootNode] ##We add a root node for all categories
    for c in cats:
        c = c.strip(' /t/n/r')
        c = c[0:c.find('(') - 1] #trim categories of ID
        categories.append(c)
    treeList = [] #list of all category trees
    currentTree = [rootNode,'','','','']
    currentdepth = 0
    
    def findLeaf(categories, i):
        if (len(categories) <=1): ##no categories passed
            return treeList
        currentDepth = getDepth(categories, i)
        newCategory = getCategory(categories, i, currentDepth)
        setCategory(currentTree, currentDepth, newCategory)
        if isLeaf(i):
            categories.pop(i) #remove the leaf node from categories list
            newTree = cleanTree(currentTree, currentDepth)
            treeList.append(newTree)
            findLeaf(categories, i-1) #searching broader parent category for more children
        elif isNotLeaf(i):
            findLeaf(categories, i + 1) #this node not leaf node, must search for children
        
    def cleanTree(currentTree, currentDepth):
        currentTree = currentTree[1:currentDepth + 1] #start from index 1 so as not to include All-products root node
        newTree = " > ".join(currentTree)
        return newTree
        
    def getCategory(categories, i, currentDepth): #gets category at i index into categories
        thisCategory = categories[i]
        cleanCategory = thisCategory[4:] #trims category depth label
        return cleanCategory
    
    def setCategory(tree, depth, category): #sets this category at depth in currentTree
        tree[depth] = category
    
    def getDepth(categories, i): #gets the depth of this category 
        return int(categories[i][0])

    def depthOfNext(categories, i): #gets the depth of the next category listed in categories
        return getDepth(categories, i+1)

    def isLeaf(i): #determines if this node is a leaf node
        if (i == len(categories) - 1) or (getDepth(categories, i) >= depthOfNext(categories, i)):
            return True
        
    def isNotLeaf(i):
        return not isLeaf(i)
    
    findLeaf(categories, 0)
    return treeList #returns final list of all category trees
