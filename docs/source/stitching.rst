Stitching
=========


.. note::

   This project is under active development.


For example::

    import os
    on_rtd = os.environ.get('READTHEDOCS') == 'True'
    if on_rtd:
        html_theme = 'default'
    else:
        html_theme = 'nature'

.. autosummary::
   :toctree: generated

   lumache
