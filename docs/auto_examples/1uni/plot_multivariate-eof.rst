
.. DO NOT EDIT.
.. THIS FILE WAS AUTOMATICALLY GENERATED BY SPHINX-GALLERY.
.. TO MAKE CHANGES, EDIT THE SOURCE PYTHON FILE:
.. "auto_examples/1uni/plot_multivariate-eof.py"
.. LINE NUMBERS ARE GIVEN BELOW.

.. only:: html

    .. note::
        :class: sphx-glr-download-link-note

        Click :ref:`here <sphx_glr_download_auto_examples_1uni_plot_multivariate-eof.py>`
        to download the full example code

.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_examples_1uni_plot_multivariate-eof.py:


Varimax-rotated Multivariate EOF analysis
============================================

Multivariate EOF analysis with additional Varimax rotation.

.. GENERATED FROM PYTHON SOURCE LINES 7-17

.. code-block:: default



    # Load packages and data:
    import xarray as xr
    import matplotlib.pyplot as plt
    from matplotlib.gridspec import GridSpec
    from cartopy.crs import PlateCarree

    from xeofs.xarray import EOF, Rotator








.. GENERATED FROM PYTHON SOURCE LINES 18-19

Create four different dataarrayss

.. GENERATED FROM PYTHON SOURCE LINES 19-25

.. code-block:: default

    t2m = xr.tutorial.load_dataset('air_temperature')['air']
    subset1 = t2m.isel(lon=slice(0, 4))
    subset2 = t2m.isel(lon=slice(5, 14))
    subset3 = t2m.isel(lon=slice(15, 34))
    subset4 = t2m.isel(lon=slice(35, None))








.. GENERATED FROM PYTHON SOURCE LINES 26-27

Perform the actual analysis

.. GENERATED FROM PYTHON SOURCE LINES 27-39

.. code-block:: default


    mpca = EOF(
        [subset1, subset2, subset3, subset4],
        dim='time',
        norm=False,
        weights='coslat'
    )
    mpca.solve()
    rot = Rotator(mpca, n_rot=50)
    reofs = rot.eofs()
    rpcs = rot.pcs()








.. GENERATED FROM PYTHON SOURCE LINES 40-41

Plot mode 1

.. GENERATED FROM PYTHON SOURCE LINES 41-75

.. code-block:: default


    mode = 1
    proj = PlateCarree()
    kwargs = {
        'cmap' : 'RdBu',
        'vmin' : -.1,
        'vmax': .1,
        'transform': proj,
        'add_colorbar': False
    }

    fig = plt.figure(figsize=(7.3, 6))
    fig.subplots_adjust(wspace=0)
    gs = GridSpec(2, 4, figure=fig, width_ratios=[1, 2, 3, 2])
    ax = [fig.add_subplot(gs[0, i], projection=proj) for i in range(4)]
    ax_pc = fig.add_subplot(gs[1, :])

    # PC
    rpcs.sel(mode=mode).plot(ax=ax_pc)
    ax_pc.set_xlabel('')
    ax_pc.set_title('')

    # EOFs
    for i, (a, eof) in enumerate(zip(ax, reofs)):
        a.coastlines(color='.5')
        eof.sel(mode=mode).plot(ax=a, **kwargs)
        a.set_xticks([])
        a.set_yticks([])
        a.set_xlabel('')
        a.set_ylabel('')
        a.set_title('Subset {:}'.format(i+1))
    ax[0].set_ylabel('EOFs')
    fig.suptitle('Mode {:}'.format(mode))
    plt.savefig('multivariate-eof-analysis.jpg')



.. image-sg:: /auto_examples/1uni/images/sphx_glr_plot_multivariate-eof_001.png
   :alt: Mode 1, Subset 1, Subset 2, Subset 3, Subset 4
   :srcset: /auto_examples/1uni/images/sphx_glr_plot_multivariate-eof_001.png
   :class: sphx-glr-single-img






.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 0 minutes  1.263 seconds)


.. _sphx_glr_download_auto_examples_1uni_plot_multivariate-eof.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download sphx-glr-download-python

     :download:`Download Python source code: plot_multivariate-eof.py <plot_multivariate-eof.py>`



  .. container:: sphx-glr-download sphx-glr-download-jupyter

     :download:`Download Jupyter notebook: plot_multivariate-eof.ipynb <plot_multivariate-eof.ipynb>`


.. only:: html

 .. rst-class:: sphx-glr-signature

    `Gallery generated by Sphinx-Gallery <https://sphinx-gallery.github.io>`_
