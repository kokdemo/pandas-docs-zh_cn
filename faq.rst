.. currentmodule:: pandas
.. _faq:

********************************
常见问题 (FAQ)
********************************

.. _df-memory-usage:

DataFrame 的内存使用
----------------------
..
  As of pandas version 0.15.0, the memory usage of a dataframe (including
  the index) is shown when accessing the ``info`` method of a dataframe. A
  configuration option, ``display.memory_usage`` (see :ref:`options`),
  specifies if the dataframe's memory usage will be displayed when
  invoking the ``df.info()`` method.

从 0.15.0 版本开始，你可以使用 dataframe 的 ``info`` 方法来查看 dataframe （包括索引）的内存使用。
这是一个可选属性，你可以通过设置 ``display.memory_usage`` (查看 :ref:`options`) 来指定调用 ``df.info()`` 方法的时候是否显示内存使用量。

..
  For example, the memory usage of the dataframe below is shown
  when calling ``df.info()``:

举个例子，以下的 dataframe 的内存使用量在调用 ``df.info()`` 方法的时候被显示出来。

.. ipython:: python

    dtypes = ['int64', 'float64', 'datetime64[ns]', 'timedelta64[ns]',
              'complex128', 'object', 'bool']
    n = 5000
    data = dict([ (t, np.random.randint(100, size=n).astype(t))
                    for t in dtypes])
    df = pd.DataFrame(data)
    df['categorical'] = df['object'].astype('category')

    df.info()

..
  The ``+`` symbol indicates that the true memory usage could be higher, because
  pandas does not count the memory used by values in columns with
  ``dtype=object``.

加号标志表示实际的内存使用量会更高一些，因为 pandas 无法计算类型为 ``dtype=object`` 的列中的内存使用。

.. versionadded:: 0.17.1

..
  Passing ``memory_usage='deep'`` will enable a more accurate memory usage report,
  that accounts for the full usage of the contained objects. This is optional
  as it can be expensive to do this deeper introspection.

输入 ``memory_usage='deep'`` 将会启用一个更精确的内存使用报告，它将会计算内部包括对象的全部使用量。
考虑到它的性能占用，使用这个参数前请三思。

.. ipython:: python

   df.info(memory_usage='deep')

..
  By default the display option is set to ``True`` but can be explicitly
  overridden by passing the ``memory_usage`` argument when invoking ``df.info()``.

默认的显示选项是  ``True`` ，但可以在调用 ``df.info()`` 时，通过 ``memory_usage`` 进行修改。

..
  The memory usage of each column can be found by calling the ``memory_usage``
  method. This returns a Series with an index represented by column names
  and memory usage of each column shown in bytes. For the dataframe above,
  the memory usage of each column and the total memory usage of the
  dataframe can be found with the memory_usage method:

每一列的内存使用可以调用 ``memory_usage`` 方法。它将会翻译一个 Series 包括每一列的列名和以 bytes 表示的内存用量。
对于以上的 dataframe，每一列的内存使用和总体 dataframe 的内存使用都可以通过 memory_usage 方法显示出来：

.. ipython:: python

    df.memory_usage()

    # total memory usage of dataframe
    df.memory_usage().sum()

..
  By default the memory usage of the dataframe's index is shown in the
  returned Series, the memory usage of the index can be suppressed by passing
  the ``index=False`` argument:

默认情况下，dataframe 的索引的内存用量也会显示在返回的 Series 中，如不想显示它，可以使用 ``index=False`` 参数。

.. ipython:: python

    df.memory_usage(index=False)

..
  The memory usage displayed by the ``info`` method utilizes the
  ``memory_usage`` method to determine the memory usage of a dataframe
  while also formatting the output in human-readable units (base-2
  representation; i.e., 1KB = 1024 bytes).

``info`` 方法显示的内存使用量会利用 ``memory_usage`` 方法来确定 dataframe 的内存使用量，同时也会格式化输出可读的单位（二进制表示，比如 1KB = 1024 bytes）。

.. See also :ref:`Categorical Memory Usage <categorical.memory>`.
看更多 :ref:`Categorical Memory Usage <categorical.memory>`。

.. Byte-Ordering Issues
字节顺序问题
--------------------
..
  Occasionally you may have to deal with data that were created on a machine with
  a different byte order than the one on which you are running Python. To deal
  with this issue you should convert the underlying NumPy array to the native
  system byte order *before* passing it to Series/DataFrame/Panel constructors
  using something similar to the following:

你可以能偶尔的需要处理一些不同字节顺序的机器生成的数据。为了解决这个问题，你需要在将数据转为 Series/DataFrame/Panel 之前使用一些方法，将数据转换基本的 NumPy 数组为你本地系统的字节顺序。
比如下面的几个方法：

.. ipython:: python

   x = np.array(list(range(10)), '>i4') # big endian
   newx = x.byteswap().newbyteorder() # force native byteorder
   s = pd.Series(newx)

..
  See `the NumPy documentation on byte order
  <http://docs.scipy.org/doc/numpy/user/basics.byteswapping.html>`__ for more
  details.

查看 `NumPy 字节顺序<http://docs.scipy.org/doc/numpy/user/basics.byteswapping.html>`__ 获取更多信息。

.. Visualizing Data in Qt applications
在Qt 程序中可视化数据
-----------------------------------

..
  There is no support for such visualization in pandas. However, the external
  package `pandas-qt <https://github.com/datalyze-solutions/pandas-qt>`_ does
  provide this functionality.

目前在 pandas 中没有这样的支持，你可以使用第三方包 `pandas-qt <https://github.com/datalyze-solutions/pandas-qt>`_ 获取这个功能。
