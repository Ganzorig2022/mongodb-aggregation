### Pushing Elements Into Newly Created Arrays ($unwind)

> https://www.mongodb.com/docs/v3.2/reference/operator/aggregation/unwind/

1. { $unwind: "$hobbies" } unwind ni hobbies: ['..', '..'] array-g zadlaad shine document vvsgene. 2 array element bwal 2 document bolno.
2. "allHobbies" gdg shine field rvv "hobbies": ["Sports", "Cooking"] datag push-lew.

```bash
# Command
db.friends.aggregate([
    { $unwind: "$hobbies" },
    { $group: { _id: { age: "$age" }, allHobbies: { $push: "$hobbies" } } }
  ]).pretty();

# Result
[
  { _id: { age: 30 }, allHobbies: [ 'Eating', 'Data Analytics' ] },
  {
    _id: { age: 29 },
    allHobbies: [ 'Sports', 'Cooking', 'Cooking', 'Skiing' ]
  }
]
```

### Getting the Length of an Array ($size)

> https://www.mongodb.com/docs/manual/reference/operator/aggregation/size/

1.

```bash
# Command
db.friends.aggregate([
    { $project: { _id: 0, numScores: { $size: "$examScores" } } }
  ]).pretty();


# Result
[ { numScores: 3 }, { numScores: 3 }, { numScores: 3 } ]
```

### Getting the Length of an Array ($size)

> https://www.mongodb.com/docs/manual/reference/operator/aggregation/filter/

1. 60-aas deesh onootoig filter-dew.

```bash
# Command
db.friends.aggregate([
    {
      $project: {
        _id: 0,
        scores: { $filter: { input: '$examScores', as: 'sc', cond: { $gt: ["$$sc.score", 60] } } }
      }
    }
  ]).pretty();



# Result
[
  {
    scores: [ { difficulty: 6, score: 62.1 }, { difficulty: 3, score: 88.5 } ]
  },
  { scores: [ { difficulty: 2, score: 74.3 } ] },
  {
    scores: [ { difficulty: 3, score: 75.1 }, { difficulty: 6, score: 61.5 } ]
  }
]
```

### Multiple Operations to Array ($uniwnd, $project, $sort, $group)

1. examScores array-g zadlaad, sortlood
2. "name"-eer ni group-leed, "score" field-eer ni $max-iig ni olow. Garah vr dvng ni mon sortlow.

```bash
# Command
db.friends.aggregate([
    { $unwind: "$examScores" },
    { $project: { _id: 1, name: 1, age: 1, score: "$examScores.score" } },
    { $sort: { score: -1 } },
    { $group: { _id: "$_id", name: { $first: "$name" }, maxScore: { $max: "$score" } } },
    { $sort: { maxScore: -1 } }
  ]).pretty();

# Result
[
  {
    _id: ObjectId('66265d2bb2d1fcf3ae117b7b'),
    name: 'Max',
    maxScore: 88.5
  },
  {
    _id: ObjectId('66265d2bb2d1fcf3ae117b7d'),
    name: 'Maria',
    maxScore: 75.1
  },
  {
    _id: ObjectId('66265d2bb2d1fcf3ae117b7c'),
    name: 'Manu',
    maxScore: 74.3
  }
]
```
