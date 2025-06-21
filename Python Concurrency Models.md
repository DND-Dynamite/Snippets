
# Parallel Computing in Python

Yes, Python has several parallel computing capabilities, though with some limitations due to the Global Interpreter Lock (GIL). Here are the main approaches:

## Native Python Options

### 1. **Threading (`threading` module)**
- Best for: I/O-bound tasks
- Limitations: GIL prevents true CPU parallelism
```python
import threading

def task():
    print("Running in thread")

threads = [threading.Thread(target=task) for _ in range(4)]
for t in threads:
    t.start()
for t in threads:
    t.join()
```

### 2. **Multiprocessing (`multiprocessing` module)**
- Best for: CPU-bound tasks
- Bypasses GIL by using separate processes
```python
from multiprocessing import Process

def worker(num):
    print(f'Worker: {num}')

processes = [Process(target=worker, args=(i,)) for i in range(4)]
for p in processes:
    p.start()
for p in processes:
    p.join()
```

### 3. **Concurrent Futures**
- High-level interface for both threading and multiprocessing
```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# For I/O-bound
with ThreadPoolExecutor() as executor:
    results = executor.map(my_io_function, data)

# For CPU-bound
with ProcessPoolExecutor() as executor:
    results = executor.map(my_cpu_function, data)
```

## Advanced/External Libraries

### 4. **Dask**
- Parallel computing with larger-than-memory datasets
- Works with NumPy, Pandas, and scikit-learn
```python
import dask.array as da

x = da.random.random((10000, 10000), chunks=(1000, 1000))
y = x + x.T
result = y.compute()  # Executes in parallel
```

### 5. **Ray**
- Distributed computing framework
- Good for machine learning workloads
```python
import ray

@ray.remote
def parallel_function(x):
    return x * x

results = ray.get([parallel_function.remote(i) for i in range(4)])
```

### 6. **Joblib**
- Simple parallel execution, especially for scikit-learn
```python
from joblib import Parallel, delayed

results = Parallel(n_jobs=4)(delayed(process)(item) for item in data)
```

### 7. **MPI (via mpi4py)**
- Message Passing Interface for HPC
```python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()
print(f"Hello from process {rank}")
```

## GIL Workarounds

### 8. **Cython with nogil**
- Release GIL in C extensions
```cython
with nogil:
    # C code that doesn't need Python API
```

### 9. **NumPy/SciPy**
- Many operations release GIL internally
```python
import numpy as np
a = np.random.rand(10000, 10000)
b = np.linalg.inv(a)  # Runs in parallel internally
```

## Limitations
- The GIL prevents true thread parallelism for CPU-bound Python code
- Multiprocessing has higher overhead than threading
- Shared memory requires special handling (multiprocessing.Array, etc.)

For most applications, `multiprocessing` or `concurrent.futures` provide the easiest path to parallelism in Python. For scientific computing, Dask and Ray offer more sophisticated options.