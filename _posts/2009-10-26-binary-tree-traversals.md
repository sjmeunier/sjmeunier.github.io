---
layout: post
title:  "Binary Tree Traversals"
date:   2009-10-26 00:00:02
categories: Programming
tags: [c-sharp, tutorials, algorithms]
---

In my last post I created a [binary tree class](/programming/2009/10/26/an-implementation-of-a-binary-tree-in-csharp.html). This post follows on from that, so I would recommend reading that first, if you have not already done so.

There are three types of traversals that are generally used to traverse a tree. These are preorder, inorder and postorder traversals, and all are similar to each other, except for the order in which the operations are performed.

For the preorder case, we first get the value of the node (starting at the root), and then recursively move down the left branch of the tree, and the recursively move down the right branch of the tree.

{% highlight csharp %}
public string preOrderTraversal(int nodeId)
{
	string str = "";

	str += (string)(((BinaryNode)nodes[nodeId]).getValue());
	if (((BinaryNode)nodes[nodeId]).getLeftChildId() != -1)
	{
		str += preOrderTraversal(((BinaryNode)nodes[nodeId]).getLeftChildId());
	}
	if (((BinaryNode)nodes[nodeId]).getRightChildId() != -1)
	{
		str += preOrderTraversal(((BinaryNode)nodes[nodeId]).getRightChildId());
	}
	return str;
}
{% endhighlight %}
<!--more-->

The inorder traversal first recurses along the left branch, then gets the value of the node, and then recurses along the right branch. I find that this tends to be the most useful of the various traversals. It is properly bottom-up, where it works it's ways to the bottom of the tree before it starts returning values.

{% highlight csharp %}        
public string inOrderTraversal(int nodeId)
{
	string str = "";

	if (((BinaryNode)nodes[nodeId]).getLeftChildId() != -1)
	{
		str += inOrderTraversal(((BinaryNode)nodes[nodeId]).getLeftChildId());
	}
	str += (string)(((BinaryNode)nodes[nodeId]).getValue());
	if (((BinaryNode)nodes[nodeId]).getRightChildId() != -1)
	{
		str += inOrderTraversal(((BinaryNode)nodes[nodeId]).getRightChildId());
	}
	return str;
}
{% endhighlight %}

The postorder traversal, predictably, first recurses the left and then the right branch, and lastly gets the value of the node.

{% highlight csharp %}
        public string postOrderTraversal(int nodeId)
        {
            string str = "";

            if (((BinaryNode)nodes[nodeId]).getLeftChildId() != -1)
            {
                str += postOrderTraversal(((BinaryNode)nodes[nodeId]).getLeftChildId());
            }
            if (((BinaryNode)nodes[nodeId]).getRightChildId() != -1)
            {
                str += postOrderTraversal(((BinaryNode)nodes[nodeId]).getRightChildId());
            }
            str += (string)(((BinaryNode)nodes[nodeId]).getValue());
            return str;
        }
{% endhighlight %}

Each of these traversals gives a different ordering of the values in the tree, and are applicable in different circumstances.

_Originally posted on my old blog, Smoky Cogs, on 26 Oct 2009_
