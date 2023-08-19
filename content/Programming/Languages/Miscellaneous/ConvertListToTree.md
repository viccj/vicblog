---
title: "Convert List To Tree"
date: 2023-08-19T08:25:41+08:00
weight: 1
tags:
- tree
- php
---

Write a programe take string with dilemeter, and write two funtions

1. input: Save input string into a collection.
2. Output: output appearence rate by givin string




```injectablephp
class TreeNode {
    public $header;
    public $count = 0;
    public $children = [];

    public function __construct($header) {
        $this->header = $header;
    }
}

class TokenCollection {
    private $tokens = [];
    private $root;

    public function __construct() {
        $this->root = new TreeNode("root");
    }

    public function ingest($string) {
        array_push($this->tokens, $string);

        $node = $this->root;

        foreach (explode(":", $string) as $val) {
            $isNew = true;

            // Check if current token has existing child nodes
            foreach ($node->children as $existingNode) {
                if ($existingNode->header === $val) {
                    $node = $existingNode; // Update the current node to the existing child node
                    $isNew = false;
                    break;
                }
            }

            if ($isNew) {
                // Create a new child node if the token is new
                $newNode = new TreeNode($val);
                $node->children[] = $newNode; // Add the new child node to the current node's children
                $node = $newNode; // Update the current node to the new child node
            }

            $node->count++; // Increment the count for the current node
        }
    }

    public function appearance($prefix) {
        $matchingCount = 0;
        $totalCount = count($this->tokens);
        
        // Iterate through stored tokens to count matching tokens
        foreach ($this->tokens as $token) {
            if (strpos($token, $prefix) === 0) {
                $matchingCount++;
            }
        }

        // Calculate and return the appearance percentage
        return $totalCount > 0 ? $matchingCount / $totalCount : 0;
    }
}

// Example usage
$collection = new TokenCollection();

$collection->ingest('a:b:c');
$collection->ingest('a:f:design');
$collection->ingest('a:f:g');
$collection->ingest('a:f:x');
$collection->ingest('d');

echo $collection->appearance('a') . "\n";  // Output: > 0.8
echo $collection->appearance('a:f') . "\n";  // Output: > 0.6

$collection->ingest('d:g:m:x');
$collection->ingest('d:g:ww');
$collection->ingest('d:g:ccc');
$collection->ingest('d:o:g');
$collection->ingest('d:o:g');

echo $collection->appearance('d:o') . "\n";  // Output: > 0.2


```
## time complexity

### ingest

time complexity of the ingest method can be represented as O(k * n * m), where:

k is the number of tokens in the string

n is the number of nodes in TokenCollection

m is the average number of child nodes in TreeNode

### appearance

The time complexity of the appearance method is O(t * k), where:

t is the number of stored tokens
k is the number of matching tokens

## space complexity

TokenCollection stores all tokens, so its space complexity is O(t), 
where t is the number of stored tokens.
TreeNode stores the tree structure, 
and its space complexity depends on the number of
nodes and the average number of child nodes. 
If there are many repetitive tokens, there might be more child nodes.
