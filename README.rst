==========================================
Yet Another Dependency Injection Container
==========================================

|travis| |coverage|

Usage example:

.. code-block:: yaml

     engine:
      Diesel:
        __realization__: yadic.examples.domain.engine.Diesel

    vehicle:
      Truck:
        __realization__: yadic.examples.domain.vehicles.Truck
        engine: Diesel

    city:
      __default__:
        __type__: static

      Paris:
        __realization__: yadic.examples.domain.address.Paris

      Fleeblebrox:
        __realization__: yadic.examples.domain.address.Fleeblebrox

    stuff:
      __default__:
        __realization__: builtins.dict

      food:
        $name: Erkburgles
      fuel:
        $name: Unobtaineum
      drink:
        $name: Nuke-Cola

    transfer:
      from_Paris_with_love:
        __realization__: yadic.examples.domain.transfer.Transfer
        __type__: singleton

        vehicle: Truck
        from_city:city: Paris
        to_city:city: Fleeblebrox
        cargo:stuff:
          - food
          - drink

.. code-block:: python

    import yaml
    from yadic.container import Container

    if __name__ == '__main__':
        with open('agent.yaml', 'r') as f:
            obj = yaml.load(f)

            cont = Container(obj)

            tr = cont.get('transfer', 'from_Paris_with_love')


    # this will be equal to

    Transfer(
        vehicle=Truck(engine=Diesel()),
        from_city=Paris,
        to_city=Fleeblebrox,
        cargo=[{'name': 'Erkburgles'}, {'name': 'Nuke-Cola'}]
    )

For more info go to `https://github.com/astynax/yadic <https://github.com/astynax/yadic>`_

.. |travis| image:: https://travis-ci.org/barsgroup/yadic.svg?branch=master
    :target: https://travis-ci.org/barsgroup/yadic
    :alt: Tests

.. |coverage| image:: https://img.shields.io/coveralls/barsgroup/yadic.svg?style=flat
    :target: https://coveralls.io/r/barsgroup/yadic
    :alt: Coverage

