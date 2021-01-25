---
title: "YUP: Validation 2 fields dependent each other"
timestamp: 2021-01-25T22:53:03.455Z
---

The main issue I found at finding this solution is the error triggered if you forgot to add the array's array parameter after the shape ([['longitude', 'latitude']] in this case).

```js
yup.object().shape(
    {
      longitude: yup
        .number()
        .nullable()
        .transform((value, originalValue) =>
          originalValue.trim() === '' ? null : value
        )
        .when(['latitude'], {
          is: (longitude, latitude) => !!longitude || !!latitude,
          then: yup
            .number()
            .typeError(t('validation.number.error'))
            .min(-180)
            .max(180)
            .required(t('validation.required.error')),
        }),
      latitude: yup
        .number()
        .nullable()
        .transform((value, originalValue) =>
          originalValue.trim() === '' ? null : value
        )
        .when(['longitude'], {
          is: (longitude, latitude) => !!longitude || !!latitude,
          then: yup
            .number()
            .typeError(t('validation.number.error'))
            .min(-90)
            .max(90)
            .required(t('validation.required.error')),
        }),
      pinpoint: yup.mixed(),
      banner: yup.mixed(),
    },
    [['longitude', 'latitude']]
  )
```
