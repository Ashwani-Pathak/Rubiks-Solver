# Rubiks-Solver

A comprehensive C++ implementation of a Rubik's Cube solver featuring multiple solving algorithms and cube representations. This project demonstrates various search algorithms and optimization techniques for solving the classic 3x3x3 Rubik's Cube.

**Author:** Ashwani Pathak  
**Email:** ashwanipathak2004@kgpian.iitkgp.ac.in

## 🎯 Project Overview

This Rubik's Cube solver implements multiple approaches to solve the 3x3x3 Rubik's Cube efficiently:

- **Multiple Cube Representations**: 3D Array, 1D Array, and Bitboard implementations
- **Various Solving Algorithms**: DFS, BFS, IDDFS, and IDA* with pattern databases
- **Heuristic-based Solving**: Uses corner pattern databases for optimal solutions
- **Comprehensive Testing**: Extensive test cases and benchmarking capabilities

## 🏗️ Architecture

### Project Structure

```
Rubiks-Solver/
├── Model/                          # Cube representation implementations
│   ├── RubiksCube.h               # Base abstract class
│   ├── RubiksCube.cpp             # Base class implementation
│   ├── RubiksCube3dArray.cpp      # 3D array representation
│   ├── RubiksCube1dArray.cpp      # 1D array representation
│   └── RubiksCubeBitboard.cpp     # Bitboard representation
├── Solver/                         # Solving algorithms
│   ├── DFSSolver.h                # Depth-First Search solver
│   ├── BFSSolver.h                # Breadth-First Search solver
│   ├── IDDFSSolver.h              # Iterative Deepening DFS solver
│   └── IDAstarSolver.h            # IDA* with pattern databases
├── PatternDatabases/              # Heuristic functions and databases
│   ├── PatternDatabase.h          # Base pattern database class
│   ├── CornerPatternDatabase.h    # Corner pattern database
│   ├── CornerDBMaker.h            # Database generation tool
│   ├── PermutationIndexer.h       # Permutation indexing utilities
│   └── NibbleArray.h              # Memory-efficient array implementation
├── Databases/                     # Pre-computed pattern databases
├── main.cpp                       # Main application and testing
├── CMakeLists.txt                 # Build configuration
└── README.md                      # This file
```

## 🔧 Cube Representations

### 1. 3D Array Representation (`RubiksCube3dArray`)
- Uses a 3D array `int cube[6][3][3]` to represent the cube
- Each face is a 3x3 grid with color values
- Intuitive and easy to understand
- Good for debugging and visualization

### 2. 1D Array Representation (`RubiksCube1dArray`)
- Uses a 1D array `int cube[54]` to represent all faces
- More memory efficient than 3D array
- Faster access patterns for certain operations
- Optimized for performance

### 3. Bitboard Representation (`RubiksCubeBitboard`)
- Uses bitboards for ultra-fast operations
- Most memory and computationally efficient
- Best performance for solving algorithms
- Recommended for production use

## 🧮 Solving Algorithms

### 1. Depth-First Search (DFS)
- **File**: `Solver/DFSSolver.h`
- **Complexity**: O(b^d) where b is branching factor, d is depth
- **Use Case**: Shallow searches, testing small configurations
- **Limitations**: Can get stuck in deep branches

### 2. Breadth-First Search (BFS)
- **File**: `Solver/BFSSolver.h`
- **Complexity**: O(b^d)
- **Use Case**: Finding shortest path solutions
- **Advantage**: Guarantees optimal solution length
- **Limitation**: Memory intensive for deep searches

### 3. Iterative Deepening DFS (IDDFS)
- **File**: `Solver/IDDFSSolver.h`
- **Complexity**: O(b^d)
- **Use Case**: When optimal solution is required but BFS uses too much memory
- **Advantage**: Combines benefits of DFS and BFS

### 4. IDA* with Pattern Databases
- **File**: `Solver/IDAstarSolver.h`
- **Complexity**: O(b^d) with effective pruning
- **Use Case**: Optimal solving with heuristic guidance
- **Features**: 
  - Uses corner pattern databases as heuristic
  - Iterative deepening with A* search
  - Most efficient for finding optimal solutions

## 🎯 Pattern Databases

### Corner Pattern Database
- **Purpose**: Provides admissible heuristic for corner positions
- **Implementation**: `PatternDatabases/CornerPatternDatabase.h`
- **Size**: Pre-computed database of corner positions and their optimal move counts
- **Usage**: Used by IDA* solver for heuristic estimation

### Database Generation
- **Tool**: `CornerDBMaker` class
- **Process**: BFS from solved state to generate all corner positions
- **Output**: Binary file with move counts for each corner configuration

## 🚀 Getting Started

### Prerequisites
- CMake 3.20 or higher
- C++14 compatible compiler (GCC, Clang, or MSVC)
- Git

### Building the Project

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Ashwani-Pathak/Rubiks-Solver
   cd Rubiks-Solver
   ```

2. **Create build directory:**
   ```bash
   mkdir build
   cd build
   ```

3. **Configure and build:**
   ```bash
   cmake ..
   make
   ```

4. **Run the solver:**
   ```bash
   ./rubiks_cube_solver
   ```

### Usage Examples

#### Basic Cube Operations
```cpp
#include "Model/RubiksCubeBitboard.h"

RubiksCubeBitboard cube;
cube.print();  // Display the solved cube

// Perform moves
cube.u();      // Up face clockwise
cube.r();      // Right face clockwise
cube.fPrime(); // Front face counter-clockwise

// Check if solved
if (cube.isSolved()) {
    cout << "Cube is solved!" << endl;
}
```

#### Using Solvers
```cpp
#include "Solver/IDAstarSolver.h"

// Create a shuffled cube
RubiksCubeBitboard cube;
auto shuffleMoves = cube.randomShuffleCube(10);

// Solve using IDA*
IDAstarSolver<RubiksCubeBitboard, HashBitboard> solver(cube, "path/to/cornerDB.txt");
auto solution = solver.solve();

// Print solution
for (auto move : solution) {
    cout << cube.getMove(move) << " ";
}
```

## 📊 Performance Characteristics

### Algorithm Comparison
| Algorithm | Optimal Solution | Memory Usage | Speed | Best For |
|-----------|-----------------|--------------|-------|----------|
| DFS       | ❌ No           | Low          | Fast  | Testing  |
| BFS       | ✅ Yes          | High         | Slow  | Short solutions |
| IDDFS     | ✅ Yes          | Low          | Medium| Optimal solutions |
| IDA*      | ✅ Yes          | Low          | Fast  | Production use |

### Representation Performance
| Representation | Memory | Speed | Debugging | Recommendation |
|----------------|--------|-------|-----------|----------------|
| 3D Array       | High   | Slow  | Easy      | Development    |
| 1D Array       | Medium | Medium| Medium    | General use    |
| Bitboard       | Low    | Fast  | Hard      | Production     |

## 🔍 Technical Details

### Move Notation
The solver uses standard Rubik's Cube notation:
- **F, F', F2**: Front face moves
- **B, B', B2**: Back face moves
- **U, U', U2**: Up face moves
- **D, D', D2**: Down face moves
- **L, L', L2**: Left face moves
- **R, R', R2**: Right face moves

### Color Encoding
- **WHITE (0)**: Up face
- **GREEN (1)**: Left face
- **RED (2)**: Front face
- **BLUE (3)**: Right face
- **ORANGE (4)**: Back face
- **YELLOW (5)**: Down face

### Hash Functions
Each cube representation includes optimized hash functions for efficient storage in hash tables:
- `Hash3d`: For 3D array representation
- `Hash1d`: For 1D array representation
- `HashBitboard`: For bitboard representation

## 🧪 Testing and Validation

The project includes comprehensive testing capabilities:
- **Move validation**: Ensures all moves are correctly implemented
- **Solution verification**: Validates that solutions actually solve the cube
- **Performance benchmarking**: Compares different algorithms and representations
- **Pattern database testing**: Verifies heuristic accuracy

## 🔧 Customization

### Adding New Solvers
1. Create a new solver class in the `Solver/` directory
2. Implement the required interface
3. Add to `CMakeLists.txt`
4. Test with various cube configurations

### Extending Cube Representations
1. Inherit from `RubiksCube` base class
2. Implement all virtual methods
3. Add appropriate hash function
4. Update build configuration

## 📈 Future Enhancements

- [ ] Edge pattern databases for even better heuristics
- [ ] Multi-threaded solving for parallel processing
- [ ] GUI interface for cube visualization
- [ ] Support for other cube sizes (2x2x2, 4x4x4, etc.)
- [ ] Machine learning-based heuristics
- [ ] Web interface for remote solving

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📄 License

This project is open source. Feel free to use, modify, and distribute according to your needs.

## 🙏 Acknowledgments

- Original implementation by Shubham Patil and Lakshya Mittal
- Pattern database concept from Korf's research
- Rubik's Cube notation standards from the speedcubing community

---

**Note**: This solver is designed for educational and research purposes. For speedcubing, consider using established methods like CFOP, Roux, or ZZ.