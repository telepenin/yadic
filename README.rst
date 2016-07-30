==========================================
Yet Another Dependency Injection Container
==========================================

|travis| |coverage|

Usage example:

.. code-block:: yaml

    # Available engines
    engine:
      Diesel:
        __is: yadic.examples.domain.engine.Diesel

    # Available vehicles
    vehicle:
      Truck:
        __is: yadic.examples.domain.vehicles.Truck

        # this will be a constructor argument
        # and value will contain an instance of Diesel class
        engine: Diesel

    # Cities
    city:
      __default__:
        __type__: static # City won't be instantiated on injection

      Paris:
        __is: yadic.examples.domain.address.Paris

      Fleeblebrox:
        __is: yadic.examples.domain.address.Fleeblebrox

    # Cargo
    stuff:
      __default__:
        __is: builtins.dict # target is just a dict

      food:
        # at this time $name is just plain kwarg (not an injection)
        $name: Erkburgles
      fuel:
        $name: Unobtaineum
      drink:
        $name: Nuke-Cola

    # transfers
    transfer:
      from_Paris_with_love:
        __is: yadic.examples.domain.transfer.Transfer
        __type__: singleton # every trasfer is unique

        vehicle: Truck

        # at this time names of kwargs differ from the name of group ("city")
        from_city:city: Paris
        to_city:city: Fleeblebrox
        cargo:stuff:
          - food
          - drink




.. code-block:: python

    import yaml
    from yadic.container import Container

    if __name__ == '__main__':
        with open('container.yaml', 'r') as f:
            cont = Container(yaml.load(f))
            tr = cont.get('transfer', 'from_Paris_with_love')


    # this will be equal to

    Transfer(
        vehicle=Truck(engine=Diesel()),
        from_city=Paris,
        to_city=Fleeblebrox,
        cargo=[{'name': 'Erkburgles'}, {'name': 'Nuke-Cola'}]
    )

It works on python 3.5, for python < 3 just need change `builtins` to `__builtin__`

For more info go to `https://github.com/astynax/yadic <https://github.com/astynax/yadic>`_

.. |travis| image:: https://travis-ci.org/barsgroup/yadic.svg?branch=master
    :target: https://travis-ci.org/barsgroup/yadic
    :alt: Tests

.. |coverage| image:: https://img.shields.io/coveralls/barsgroup/yadic.svg?style=flat
    :target: https://coveralls.io/r/barsgroup/yadic
    :alt: Coverage

