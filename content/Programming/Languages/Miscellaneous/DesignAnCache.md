---
title: "Design An Cache"
date: 2023-08-08T08:25:41+08:00
weight: 6
tags:
- cache
- php
---

```injectablephp
class Cache {
    private $capacity;   // Capacity of the cache
    private $data = [];  // Data storage for key-value pairs
    private $scores = []; // Scores storage for keys

    public function __construct($capacity) {
        $this->capacity = $capacity;  // Initialize cache with given capacity
    }

    public function get($key) {
        if (isset($this->data[$key])) {
            $this->updateTimestamp($key);  // Update timestamp to reflect recent access
            return $this->data[$key][0];   // Return the value associated with the key
        }
        return -1;  // Return -1 if key is not found
    }

    public function put($key, $value, $weight) {
        if (count($this->data) >= $this->capacity) {
            $this->evict(); // Perform eviction if cache capacity is reached
        }
        $this->data[$key] = [$value, $weight, time()]; // Store key-value, weight, and timestamp
        $score = $this->calculateScore($weight, $this->data[$key][2]); // Calculate score
        $this->updateScore($key, $score); // Update score for the key
    }

    private function updateTimestamp($key) {
        $this->data[$key][2] = time(); // Update the timestamp for the key
    }

    private function evict() {
        $minKey = array_search(min($this->scores), $this->scores); // Find key with the minimum score
        unset($this->data[$minKey], $this->scores[$minKey]); // Remove the key from data and scores
    }

    private function updateScore($key, $score) {
        $this->scores[$key] = $score; // Update the score for the key
    }

    private function calculateScore($weight, $timestamp) {
        $currentTime = time();  // Get the current time
        $timeDifference = max(1, $currentTime - $timestamp); // Calculate time difference
        return $weight / (log($timeDifference) + 1); // Calculate and return the score
    }
}


```

## time complexity
O(1) for both get and put method

## space complexity
O(n)