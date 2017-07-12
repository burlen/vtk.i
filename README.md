# vtk.i
A SWIG interface for VTK.

## Rationale
This interface is useful when your project uses SWIG to generate it's Python API and
1. your API contains VTK objects that will be wrapped by VTK's wrapper generator
2. any of your classes derive from VTK objects and thus are reference counted using VTK's reference counting implementation

## Usage
The interface comes in the form of a number of macros. One uses the macros to enable SWIG to make use of VTK's reference counting implementation and to expose your classes which are derived from a VTK class.

In order to use include `vtk.i` into your SWIG .i file and call one of the macros below that expose VTK objects to SWIG.
```bash
%include <vtk.i>
```

### VTK_SWIG_INTEROP
```bash
VTK_SWIG_INTEROP(vtk_t)
```
arguments:
```bash
vtk_t - a VTK class name that is used in the SWIG generated API.
```
The macro defines the typemaps needed for SWIG to convert to and
from VTK's Python bindings. Use this when your API containes pointers
to classes defined in VTK.

For example if your API makes use of vtkObjectBase and vtkDataObject your SWIG .i file would include:
```bash
VTK_SWIG_INTEROP(vtkObjectBase)
VTK_SWIG_INTEROP(vtkDataObject)
```
There will be one macro invokation per VTK class appearing in your API.

###  VTK_DERIVED
```bash
VTK_DERIVED(derived_t)
```
arguments:
```bash
derived_t - name of a class that derives from vtkObjectBase.
```
The macro causes SWIG to wrap the class and defines memory management hooks
that prevent memory leaks when SWIG creates the objects. Use this to wrap
VTK classes defined in your project.

For example if you define a class derived from vtkObject or one of its subclasses called `DataAdaptor` your SWIG .i file would include:
```bash
VTK_DERIVED(DataAdaptor)
```
Note that you will also need to call `VTK_SWIG_INTEROP` for any VTK classes in your class's API including vtkObjectBase.

## Examples
The [SENSEI](https://gitlab.kitware.com/sensei/sensei) project makes use of these macros. Specifically in [senseiPython.i](https://gitlab.kitware.com/sensei/sensei/blob/master/python/senseiPython.i).
