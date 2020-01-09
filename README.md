# Nodes

Implementación de prueba de hacker rank 
https://www.hackerrank.com/contests/algoritmos-y-estructuras-de-datos/challenges/arbol-de-busqueda-binaria/submissions/code/1319158071

TODO: Comments


nodes implementation
//La primer línea tendrá el valor de n (cantidad de números enteros en la lista), la segunda línea tendrá la lista de n números enteros separados por espacios, la tercer línea tendrá el valor de m (cantidad de preguntas), las siguientes m líneas tendrán los valores enteros que se desean buscar en el árbol:
function processData(input) {
    //Enter your code here
    data = input.split('\n')
    inputData = data[1].split(' ')
    tree = populate(inputData, data[0])
    numQuestions = data[2]
    result = []
    for (i = 0; i < numQuestions; i++) {
        if (tree.search(data[i+3])) {
            result.push(1)
        } else {
            result.push(0)
        }
    }
    queryResult = result.toString().replace(/\,/g, '\n')
    
    
    order = tree.orderElements().toString().replace(/\,/g, ' ')
    preOrder = tree.preorderElements().toString().replace(/\,/g, ' ')
    postOrder = tree.postorderElements().toString().replace(/\,/g, ' ')

    finalResult = queryResult+'\n'+order+'\n'+preOrder+'\n'+postOrder
    console.log(finalResult)
} 

function populate(data, dataLenght) {
    tree = new Tree()
    for (i = 0; i < dataLenght; i++) {
        value = data[i]
        tree.insert(value)
    }
    return tree
}

class Node {
    constructor(value) {
        this.value = parseInt(value)
        this.left = null
        this.right = null
    }
}
class Tree {
    constructor() {
        this.root = null
        this.order = []
        this.preorder = []
        this.postorder = []
    }
    
    insert(value) {
        let node = new Node(value)
        // this is for root node :)
        if (this.root === null) {
            this.root = node
            return this
        }
        // lets find childs
        this.addNode(this.root, node)
        return this
    }

    addNode(oldNode, newNode) {
        if (oldNode.value > newNode.value) {
            if (oldNode.left == null) {
                oldNode.left = newNode
            } else {
                this.addNode(oldNode.left, newNode)
            }
        } else {
            if (oldNode.right == null) {
                oldNode.right = newNode
            } else {
                this.addNode(oldNode.right, newNode)
            }
        }
    }
    
    search(value) {
        return this.findElement(this.root, value)
    }

    findElement(node, value) {
        if (node === null) {
            return null
        }
        
        if (node.value < value) {
            return this.findElement(node.right, value)
        }

        if (node.value > value) {
            return this.findElement(node.left, value)
        }
        
        return node
    }

    orderElements() {
        this.fetchOrderElements(this.root)
        return this.order
    }
    
    preorderElements() {
        this.fetchPreorderElements(this.root)
        return this.preorder
    }
    
    postorderElements() {
        this.fetchPostorderElements(this.root)
        return this.postorder
    }
    
    fetchOrderElements(node) {
        if (node != null) {
            this.fetchOrderElements(node.left)
            this.order.push(node.value)
            this.fetchOrderElements(node.right)    
        }
    }

    fetchPreorderElements(node) {        
        if (node != null) {
            this.preorder.push(node.value)
            this.fetchPreorderElements(node.left)
            this.fetchPreorderElements(node.right)    
        }
    }
    
    fetchPostorderElements(node) {
        if (node != null) {
            this.fetchPostorderElements(node.left)
            this.fetchPostorderElements(node.right)    
            this.postorder.push(node.value)
        }
    }
}

process.stdin.resume();
process.stdin.setEncoding("ascii");
_input = "";
process.stdin.on("data", function (input) {
    _input += input;
});

process.stdin.on("end", function () {
   processData(_input);
});
