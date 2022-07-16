# dockerization
Better organization for all my dockerization endeavors.

# Ecoding
Directories containing images are prefixed with an encoding scheme that identifies the entire dependency tree.

The number in the prefix identifies what layer image it is in the tree.
The character after the number indicates what image variant it is at that layer.
The first letters of the prefix indicates the image variant at each of the prior layers.
