---
layout: post
title:  "An Implementation of a Binary Tree in C#"
date:   2009-10-26 00:00:01
categories: Programming
tags: [c-sharp, tutorials, algorithms]
---

Binary trees are a very useful data structure, and are used in things like data storage, and parsing of mathematical expressions. Implementing a binary tree is C# is not hard at all.

The first thing we need to do is define a class for the node of the tree. **BinaryNode**. This contains four variables: **value** which is an object containing whatever data you have set the node to. **leftChildId** and **rightChildId** are the ids of the child nodes linked to this node. **parentId** is the id pf the parent node to this node, and is not strictly necessary, but it does make modifying the tree much simpler - and more efficient.

The rest of the class is purely functions to get and set these values.
<!--more-->

{% highlight csharp %}
namespace BinaryTree
{
    public class BinaryNode
    {
        private object value;
        private int leftChildId = -1;
        private int rightChildId = -1;
        private int parentId = -1;

        public void setLeftChildId(int nodeId)
        {
            this.leftChildId = nodeId;
        }

        public int getLeftChildId()
        {
            return this.leftChildId;
        }

        public void setRightChildId(int nodeId)
        {
            this.rightChildId = nodeId;
        }

        public int getRightChildId()
        {
            return this.rightChildId;
        }

        public void setParentId(int nodeId)
        {
            this.parentId = nodeId;
        }

        public int getParentId()
        {
            return this.parentId;
        }

        public void setValue(object val)
        {
            this.value = val;
        }

        public object getValue()
        {
            return this.value;
        }

    }
}
{% endhighlight %}

Next up we have the binary tree class, which has an arraylist, which gets populated with the nodes.

When a new tree is created, the first node - the root - is automatically created. This is done by creating a new instance of a **BinaryNode**, setting the value, and then adding it to the arraylist.

We then have functions to add nodes to the left or the right of the node - **addLeftNode** and **addRightNode**.

These functions create a new node, set the value and parent id, and then add the node to the arraylist, returning back the id of the new node. Then we assign the new id to either the left or right child value of the parent node.

The next functions get and set the value of a particular node.

The **removeNode** function removes a node from the tree, as well as all children nodes attached to it. It does this recursively, moving down the tree and deleting nodes as it goes upwards until it reaches the actual node we specified, which gets deleted last. The parent node's child id then gets set to -1, which indicated that there is not longer a child attached.

The following two functions, **setLeftChildNode** and **setRightChildNode** takes an existing child node, and moves it from its current position and reattaches it to the specified node on either the left or right side depending on the function called.

Now we should have a fully operational tree, which you can add and remove nodes to, as well as getting and setting the values of the nodes.

{% highlight csharp %}
using System;
using System.Collections;

namespace BinaryTree
{
    public class BinaryTree
    {
        private ArrayList nodes = new ArrayList();

        public BinaryTree()
        {
            BinaryNode newNode = new BinaryNode();
            nodes.Add(newNode);
        }

        public BinaryTree(object val)
        {
            BinaryNode newNode = new BinaryNode();
            newNode.setValue(val);
            nodes.Add(newNode);
        }

        public int addLeftNode(object val, int parentId)
        {
            BinaryNode newNode = new BinaryNode();
            newNode.setValue(val);
            newNode.setParentId = parentId;
            int nodeId = nodes.Add(newNode);

            ((BinaryNode)nodes[parentId]).setLeftChildId(nodeId);
            return nodeId;
        }

        public int addRightNode(object val, int parentId)
        {
            BinaryNode newNode = new BinaryNode();
            newNode.setValue(val);
            newNode.setParentId = parentId;
            int nodeId = nodes.Add(newNode);
            ((BinaryNode)nodes[parentId]).setRightChildId(nodeId);
            return nodeId;
        }

        public object getNodeValue(int nodeId)
        {
            return ((BinaryNode)nodes[nodeId]).getValue();
        }

        public void setNodeValue(int nodeId, object val)
        {
            ((BinaryNode)nodes[nodeId]).setValue(val);
        }

        public void removeNode(int nodeId)
        {
            //recurse to remove children
            if (((BinaryNode)nodes[nodeId]).getLeftChildId() != -1)
            {
                removeNode(((BinaryNode)nodes[nodeId]).getLeftChildId());
            }
            if (((BinaryNode)nodes[nodeId]).getRightChildId() != -1)
            {
                removeNode(((BinaryNode)nodes[nodeId]).getRightChildId());
            }

            //remove parent reference
            int oldParentId = ((BinaryNode)nodes[nodeId]).getParentId();
            if (((BinaryNode)nodes[oldParentId]).getLeftChildId() == nodeId)
            {
                ((BinaryNode)nodes[oldParentId]).setLeftChildId(-1);
            }
            if (((BinaryNode)nodes[oldParentId]).getRightChildId() == nodeId)
            {
                ((BinaryNode)nodes[oldParentId]).setRightChildId(-1);
            }
            nodes.RemoveAt(nodeId);
        }

        public void setLeftChildNode(int nodeId, int childId)
        {

            //Have to remove old references to the child nodeId
            int oldParentId = ((BinaryNode)nodes[childId]).getParentId();
            if (((BinaryNode)nodes[oldParentId]).getLeftChildId() == childId)
            {
                ((BinaryNode)nodes[oldParentId]).setLeftChildId(-1);
            }
            if (((BinaryNode)nodes[oldParentId]).getRightChildId() == childId)
            {
                ((BinaryNode)nodes[oldParentId]).setRightChildId(-1);
            }

            //Now assign the new node
            ((BinaryNode)nodes[nodeId]).setLeftChildId(childId);
            ((BinaryNode)nodes[childId]).setParentId(nodeId);
        }

        public void setRightChildNode(int nodeId, int childId)
        {
            //Have to remove old references to the child nodeId
            int oldParentId = ((BinaryNode)nodes[childId]).getParentId();
            if (((BinaryNode)nodes[oldParentId]).getLeftChildId() == childId)
            {
                ((BinaryNode)nodes[nodeId]).setLeftChildId(-1);
            }
            if (((BinaryNode)nodes[oldParentId]).getRightChildId() == childId)
            {
                ((BinaryNode)nodes[nodeId]).setRightChildId(-1);
            }

            //Now assign the new node
            ((BinaryNode)nodes[nodeId]).setRightChildId(childId);
            ((BinaryNode)nodes[childId]).setParentId(nodeId);
        }

    }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 26 Oct 2009_
