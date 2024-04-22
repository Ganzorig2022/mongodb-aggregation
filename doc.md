### Working with Group Stage ($group)

1. Ehleed "female" field-eer ni filter ($match) hiine.
2.

```bash
# Command
db.persons.aggregate([
    { $match: { gender: 'female' } },
    { $group: { _id: { state: "$location.state" }, totalPersons: { $sum: 1 } } },
    { $sort: { totalPersons: -1 } } # DESC order = Highest values first.
]).pretty();

# Result
[
  { _id: { state: 'midtjylland' }, totalPersons: 33 },
  { _id: { state: 'nordjylland' }, totalPersons: 27 },
  { _id: { state: 'australian capital territory' }, totalPersons: 24 },
  { _id: { state: 'syddanmark' }, totalPersons: 24 },
  { _id: { state: 'new south wales' }, totalPersons: 24 },
  { _id: { state: 'south australia' }, totalPersons: 22 },
...
]
```

### Working with Project Stage ($project)

> https://www.mongodb.com/docs/v3.2/reference/operator/aggregation-string/
> Transform every document data. Tuhain collection dotorh data-nuudiig oorchilhod heregledeg.

1. "gender" field-iig oruulaad shineer "fullName": {} nemew. Ingehdee "name": { "first": "victor", "last": "pedersen" } field-iig ashiglaw.

```bash
# Command
db.persons.aggregate([
    {
      $project: {
        _id: 0, # _id field-iig exclude
        gender: 1, # gender field-iig include
        fullName: {
          $concat: [
            { $toUpper: { $substrCP: ['$name.first', 0, 1] } },
            {
              $substrCP: [
                '$name.first',
                1,
                { $subtract: [{ $strLenCP: '$name.first' }, 1] }
              ]
            },
            ' ',
            { $toUpper: { $substrCP: ['$name.last', 0, 1] } },
            {
              $substrCP: [
                '$name.last',
                1,
                { $subtract: [{ $strLenCP: '$name.last' }, 1] }
              ]
            }
          ]
        }
      }
    }
  ]).pretty();

# Result
[
  { gender: 'male', fullName: 'Victor Pedersen' },
  { gender: 'male', fullName: 'Carl Jacobs' },
  { gender: 'male', fullName: 'Zachary Lo' },
  { gender: 'male', fullName: 'Harvey Chambers' },
  { gender: 'male', fullName: 'Gideon Van drongelen' },
  { gender: 'female', fullName: 'پریا پارسا' },
  { gender: 'female', fullName: 'Maeva Wilson' },
...
]
```
