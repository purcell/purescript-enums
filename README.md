# Module Documentation

## Module Data.Enum

#### `Cardinality`

``` purescript
newtype Cardinality a
  = Cardinality Number
```


#### `runCardinality`

``` purescript
runCardinality :: forall a. Cardinality a -> Number
```


#### `Enum`

``` purescript
class (Ord a) <= Enum a where
  cardinality :: Cardinality a
  firstEnum :: a
  lastEnum :: a
  succ :: a -> Maybe a
  pred :: a -> Maybe a
  toEnum :: Number -> Maybe a
  fromEnum :: a -> Number
```

Type class for enumerations. This should not be considered a part of a
numeric hierarchy, ala Haskell. Rather, this is a type class for small,
ordered sum types with statically-determined cardinality and the ability 
to easily compute successor and predecessor elements. e.g. `DayOfWeek`, etc.

Laws:

- ```succ firstEnum >>= succ >>= succ ... succ [cardinality - 1 times] == Just lastEnum```
- ```pred lastEnum  >>= pred >>= pred ... pred [cardinality - 1 times] == Just firstEnum```
- ```e1 `compare` e2 == fromEnum e1 `compare` fromEnum e2```
- ```forall a > firstEnum: pred a >>= succ == Just a```
- ```forall a < lastEnum:  succ a >>= pred == Just a```
- ```pred >=> succ >=> pred = pred```
- ```succ >=> pred >=> succ = succ```
- ```toEnum (fromEnum a) = Just a```
- ```forall a > firstEnum: fromEnum <$> pred a = Just (fromEnum a - 1)```
- ```forall a < lastEnum:  fromEnum <$> succ a = Just (fromEnum a + 1)```

#### `defaultSucc`

``` purescript
defaultSucc :: forall a. (Number -> Maybe a) -> (a -> Number) -> a -> Maybe a
```

```defaultSucc toEnum fromEnum = succ```

#### `defaultPred`

``` purescript
defaultPred :: forall a. (Number -> Maybe a) -> (a -> Number) -> a -> Maybe a
```

```defaultPred toEnum fromEnum = pred```

#### `defaultToEnum`

``` purescript
defaultToEnum :: forall a. (a -> Maybe a) -> a -> Number -> Maybe a
```

Runs in `O(n)` where `n` is `fromEnum a`

```defaultToEnum succ firstEnum = toEnum```

#### `defaultFromEnum`

``` purescript
defaultFromEnum :: forall a. (a -> Maybe a) -> a -> Number
```

Runs in `O(n)` where `n` is `fromEnum a`

```defaultFromEnum pred = fromEnum```

#### `enumFromTo`

``` purescript
enumFromTo :: forall a. (Enum a) => a -> a -> [a]
```

Property: ```fromEnum a = a', fromEnum b = b' => forall e', a' <= e' <= b': Exists e: toEnum e' = Just e```

Following from the propery of `intFromTo`, we are sure all elements in `intFromTo (fromEnum a) (fromEnum b)` are `Just`s.

#### `enumFromThenTo`

``` purescript
enumFromThenTo :: forall a. (Enum a) => a -> a -> a -> [a]
```

`[a,b..c]`

Correctness for using `fromJust` is the same as for `enumFromTo`.

#### `intFromTo`

``` purescript
intFromTo :: Number -> Number -> [Number]
```

Property: ```forall e in intFromTo a b: a <= e <= b```

#### `intStepFromTo`

``` purescript
intStepFromTo :: Number -> Number -> Number -> [Number]
```

Property: ```forall e in intStepFromTo step a b: a <= e <= b```

#### `enumChar`

``` purescript
instance enumChar :: Enum Char
```

## Instances

#### `enumMaybe`

``` purescript
instance enumMaybe :: (Enum a) => Enum (Maybe a)
```


#### `enumBoolean`

``` purescript
instance enumBoolean :: Enum Boolean
```


#### `enumTuple`

``` purescript
instance enumTuple :: (Enum a, Enum b) => Enum (Tuple a b)
```


#### `enumEither`

``` purescript
instance enumEither :: (Enum a, Enum b) => Enum (Either a b)
```
