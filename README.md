<a href="https://github.com/fantasyland/fantasy-land"><img alt="Fantasy Land" src="https://raw.githubusercontent.com/fantasyland/fantasy-land/master/logo.png" width="75" height="75" align="left"></a>

# sanctuary-either

The Either type represents values with two possibilities: a value of type
`Either a b` is either a Left whose value is of type `a` or a Right whose
value is of type `b`.

`Either a b` satisfies the following [Fantasy Land][] specifications:

```javascript
> const Useless = require ('sanctuary-useless')

> const isTypeClass = x =>
.   type (x) === 'sanctuary-type-classes/TypeClass@1'

> S.map (k => k + ' '.repeat (16 - k.length) +
.             (Z[k].test (Right (Useless)) ? '\u2705   ' :
.              Z[k].test (Right (['foo'])) ? '\u2705 * ' :
.              /* otherwise */               '\u274C   '))
.       (S.keys (S.unchecked.filter (isTypeClass) (Z)))
[ 'Setoid          ✅ * ',  // if ‘a’ and ‘b’ satisfy Setoid
. 'Ord             ✅ * ',  // if ‘a’ and ‘b’ satisfy Ord
. 'Semigroupoid    ❌   ',
. 'Category        ❌   ',
. 'Semigroup       ✅ * ',  // if ‘a’ and ‘b’ satisfy Semigroup
. 'Monoid          ❌   ',
. 'Group           ❌   ',
. 'Filterable      ❌   ',
. 'Functor         ✅   ',
. 'Bifunctor       ✅   ',
. 'Profunctor      ❌   ',
. 'Apply           ✅   ',
. 'Applicative     ✅   ',
. 'Chain           ✅   ',
. 'ChainRec        ✅   ',
. 'Monad           ✅   ',
. 'Alt             ✅   ',
. 'Plus            ❌   ',
. 'Alternative     ❌   ',
. 'Foldable        ✅   ',
. 'Traversable     ✅   ',
. 'Extend          ✅   ',
. 'Comonad         ❌   ',
. 'Contravariant   ❌   ' ]
```

#### <a name="Either" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L144">`Either :: TypeRep Either`</a>

Either [type representative][].

#### <a name="Either.Left" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L148">`Either.Left :: a -⁠> Either a b`</a>

Constructs a value of type `Either a b` from a value of type `a`.

```javascript
> Left ('sqrt undefined for -1')
Left ('sqrt undefined for -1')
```

#### <a name="Either.Right" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L171">`Either.Right :: b -⁠> Either a b`</a>

Constructs a value of type `Either a b` from a value of type `b`.

```javascript
> Right (42)
Right (42)
```

#### <a name="Either.fantasy-land/of" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L194">`Either.fantasy-land/of :: b -⁠> Either a b`</a>

  - `of (Either) (x)` is equivalent to `Right (x)`

```javascript
> S.of (Either) (42)
Right (42)
```

#### <a name="Either.fantasy-land/chainRec" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L207">`Either.fantasy-land/chainRec :: ((a -⁠> c, b -⁠> c, a) -⁠> Either d c, a) -⁠> Either d b`</a>

```javascript
> Z.chainRec (
.   Either,
.   (next, done, x) =>
.     x <= 1 ? Left ('!!') : Right (x >= 1000 ? done (x) : next (x * x)),
.   1
. )
Left ('!!')

> Z.chainRec (
.   Either,
.   (next, done, x) =>
.     x <= 1 ? Left ('!!') : Right (x >= 1000 ? done (x) : next (x * x)),
.   2
. )
Right (65536)
```

#### <a name="Either.prototype.@@show" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L236">`Either#@@show :: (Showable a, Showable b) => Either a b ~> () -⁠> String`</a>

  - `show (Left (x))` is equivalent to `'Left (' + show (x) + ')'`
  - `show (Right (x))` is equivalent to `'Right (' + show (x) + ')'`

```javascript
> show (Left ('sqrt undefined for -1'))
'Left ("sqrt undefined for -1")'

> show (Right ([1, 2, 3]))
'Right ([1, 2, 3])'
```

#### <a name="Either.prototype.fantasy-land/equals" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L255">`Either#fantasy-land/equals :: (Setoid a, Setoid b) => Either a b ~> Either a b -⁠> Boolean`</a>

  - `Left (x)` is equal to `Left (y)` [iff][] `x` is equal to `y`
    according to [`Z.equals`][]
  - `Right (x)` is equal to `Right (y)` [iff][] `x` is equal to `y`
    according to [`Z.equals`][]
  - `Left (x)` is never equal to `Right (y)`

```javascript
> S.equals (Left ([1, 2, 3])) (Left ([1, 2, 3]))
true

> S.equals (Right ([1, 2, 3])) (Right ([1, 2, 3]))
true

> S.equals (Left ([1, 2, 3])) (Right ([1, 2, 3]))
false
```

#### <a name="Either.prototype.fantasy-land/lte" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L280">`Either#fantasy-land/lte :: (Ord a, Ord b) => Either a b ~> Either a b -⁠> Boolean`</a>

  - `Left (x)` is less than or equal to `Left (y)` [iff][] `x` is less
    than or equal to `y` according to [`Z.lte`][]
  - `Right (x)` is less than or equal to `Right (y)` [iff][] `x` is less
    than or equal to `y` according to [`Z.lte`][]
  - `Left (x)` is always less than `Right (y)`

```javascript
> S.filter (S.lte (Left (1))) ([Left (0), Left (1), Left (2)])
[Left (0), Left (1)]

> S.filter (S.lte (Right (1))) ([Right (0), Right (1), Right (2)])
[Right (0), Right (1)]

> S.filter (S.lte (Left (1))) ([Right (0), Right (1), Right (2)])
[]

> S.filter (S.lte (Right (1))) ([Left (0), Left (1), Left (2)])
[Left (0), Left (1), Left (2)]
```

#### <a name="Either.prototype.fantasy-land/concat" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L308">`Either#fantasy-land/concat :: (Semigroup a, Semigroup b) => Either a b ~> Either a b -⁠> Either a b`</a>

  - `concat (Left (x)) (Left (y))` is equivalent to
    `Left (concat (x) (y))`
  - `concat (Right (x)) (Right (y))` is equivalent to
    `Right (concat (x) (y))`
  - `concat (Left (x)) (Right (y))` is equivalent to `Right (y)`
  - `concat (Right (x)) (Left (y))` is equivalent to `Right (x)`

```javascript
> S.concat (Left ('abc')) (Left ('def'))
Left ('abcdef')

> S.concat (Right ([1, 2, 3])) (Right ([4, 5, 6]))
Right ([1, 2, 3, 4, 5, 6])

> S.concat (Left ('abc')) (Right ([1, 2, 3]))
Right ([1, 2, 3])

> S.concat (Right ([1, 2, 3])) (Left ('abc'))
Right ([1, 2, 3])
```

#### <a name="Either.prototype.fantasy-land/map" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L337">`Either#fantasy-land/map :: Either a b ~> (b -⁠> c) -⁠> Either a c`</a>

  - `map (f) (Left (x))` is equivalent to `Left (x)`
  - `map (f) (Right (x))` is equivalent to `Right (f (x))`

```javascript
> S.map (S.add (1)) (Left ('sqrt undefined for -1'))
Left ('sqrt undefined for -1')

> S.map (S.add (1)) (Right (99))
Right (100)
```

#### <a name="Either.prototype.fantasy-land/bimap" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L356">`Either#fantasy-land/bimap :: Either a c ~> (a -⁠> b, c -⁠> d) -⁠> Either b d`</a>

  - `bimap (f) (g) (Left (x))` is equivalent to `Left (f (x))`
  - `bimap (f) (g) (Right (x))` is equivalent to `Right (g (x))`

```javascript
> S.bimap (S.toUpper) (S.add (1)) (Left ('abc'))
Left ('ABC')

> S.bimap (S.toUpper) (S.add (1)) (Right (99))
Right (100)
```

#### <a name="Either.prototype.fantasy-land/ap" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L375">`Either#fantasy-land/ap :: Either a b ~> Either a (b -⁠> c) -⁠> Either a c`</a>

  - `ap (Left (x)) (Left (y))` is equivalent to `Left (x)`
  - `ap (Left (x)) (Right (y))` is equivalent to `Left (x)`
  - `ap (Right (f)) (Left (x))` is equivalent to `Left (x)`
  - `ap (Right (f)) (Right (x))` is equivalent to `Right (f (x))`

```javascript
> S.ap (Left ('div undefined for 0')) (Left ('sqrt undefined for -1'))
Left ('div undefined for 0')

> S.ap (Left ('div undefined for 0')) (Right (99))
Left ('div undefined for 0')

> S.ap (Right (S.add (1))) (Left ('sqrt undefined for -1'))
Left ('sqrt undefined for -1')

> S.ap (Right (S.add (1))) (Right (99))
Right (100)
```

#### <a name="Either.prototype.fantasy-land/chain" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L402">`Either#fantasy-land/chain :: Either a b ~> (b -⁠> Either a c) -⁠> Either a c`</a>

  - `chain (f) (Left (x))` is equivalent to `Left (x)`
  - `chain (f) (Right (x))` is equivalent to `f (x)`

```javascript
> const sqrt = n => n < 0 ? Left ('sqrt undefined for ' + show (n))
.                         : Right (Math.sqrt (n))

> S.chain (sqrt) (Left ('div undefined for 0'))
Left ('div undefined for 0')

> S.chain (sqrt) (Right (-1))
Left ('sqrt undefined for -1')

> S.chain (sqrt) (Right (25))
Right (5)
```

#### <a name="Either.prototype.fantasy-land/alt" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L427">`Either#fantasy-land/alt :: Either a b ~> Either a b -⁠> Either a b`</a>

  - `alt (Left (y)) (Left (x))` is equivalent to `Left (y)`
  - `alt (Right (y)) (Left (x))` is equivalent to `Right (y)`
  - `alt (Left (y)) (Right (x))` is equivalent to `Right (x)`
  - `alt (Right (y)) (Right (x))` is equivalent to `Right (x)`

```javascript
> S.alt (Left ('B')) (Left ('A'))
Left ('B')

> S.alt (Right (1)) (Left ('C'))
Right (1)

> S.alt (Left ('D')) (Right (2))
Right (2)

> S.alt (Right (4)) (Right (3))
Right (3)
```

#### <a name="Either.prototype.fantasy-land/reduce" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L454">`Either#fantasy-land/reduce :: Either a b ~> ((c, b) -⁠> c, c) -⁠> c`</a>

  - `reduce (f) (x) (Left (y))` is equivalent to `x`
  - `reduce (f) (x) (Right (y))` is equivalent to `f (x) (y)`

```javascript
> S.reduce (S.concat) ([1]) (Left ('sqrt undefined for -1'))
[1]

> S.reduce (S.concat) ([1]) (Right ([2]))
[1, 2]
```

#### <a name="Either.prototype.fantasy-land/traverse" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L473">`Either#fantasy-land/traverse :: Applicative f => Either a b ~> (TypeRep f, b -⁠> f c) -⁠> f (Either a c)`</a>

  - `traverse (A) (f) (Left (x))` is equivalent to `of (A) (Left (x))`
  - `traverse (A) (f) (Right (x))` is equivalent to `map (Right) (f (x))`

```javascript
> S.traverse (Array) (S.words) (Left ('sqrt undefined for -1'))
[Left ('sqrt undefined for -1')]

> S.traverse (Array) (S.words) (Right ('foo bar baz'))
[Right ('foo'), Right ('bar'), Right ('baz')]
```

#### <a name="Either.prototype.fantasy-land/extend" href="https://github.com/sanctuary-js/sanctuary-either/blob/v2.1.0/index.js#L492">`Either#fantasy-land/extend :: Either a b ~> (Either a b -⁠> c) -⁠> Either a c`</a>

  - `extend (f) (Left (x))` is equivalent to `Left (x)`
  - `extend (f) (Right (x))` is equivalent to `Right (f (Right (x)))`

```javascript
> S.extend (S.reduce (S.add) (1)) (Left ('sqrt undefined for -1'))
Left ('sqrt undefined for -1')

> S.extend (S.reduce (S.add) (1)) (Right (99))
Right (100)
```

[Fantasy Land]:             https://github.com/fantasyland/fantasy-land/tree/v4.0.1
[`Z.equals`]:               https://github.com/sanctuary-js/sanctuary-type-classes/tree/v12.1.0#equals
[`Z.lte`]:                  https://github.com/sanctuary-js/sanctuary-type-classes/tree/v12.1.0#lte
[iff]:                      https://en.wikipedia.org/wiki/If_and_only_if
[type representative]:      https://github.com/fantasyland/fantasy-land/tree/v4.0.1#type-representatives
