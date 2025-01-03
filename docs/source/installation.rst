Installation
============

Setup Instructions
------------------

From a terminal, clone the repository:

```
git clone https://github.com/lanl/SuperNu.git supernu
```

If https protocal does not work, try ssh:

```
git clone git@github.com:lanl/SuperNu.git supernu
```

Verify checkout:

```
cd supernu
ls
```

Starting from the source directory, to build:

```
mkdir build
```
```
cd build
```
```
cmake ../supernu
```
```
ccmake .           # (optionally) change configuration options
```
make -j
```

The supernu executable should now be in the 'build' directory.


Use Instructions
----------------

Prepare sim directory:

```
mkdir -p ~/sim-supernu/test/run001
cd ~/sim-supernu/test/run001
cp ~/build/supernu .
ln -sf ~/supernu/Data/* .
ln -sf ~/supernu/Data/Atoms/* ./Atoms
```

Setup simulation:

```
cp -f ~/supernu/Input/input.str_r64 input.str
cp -f ~/supernu/Input/input.w7.par input.par
echo "in_name = 'w7_11r64'" >> input.par
echo "in_comment = 'test simulation 1D'" >> input.par
```
