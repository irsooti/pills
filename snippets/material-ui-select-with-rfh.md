---
title: "Material UI: select field with react-hook-form"
timestamp: 2021-01-21T09:30:04.078Z
---

This is the best way I've found to handle a select field with material-ui.


```jsx
import {
  FormControl,
  FormHelperText,
  InputLabel,
  MenuItem,
  Select,
} from '@material-ui/core';
import { get, kebabCase, uniqueId } from 'lodash';
import React, { useMemo } from 'react';
import { Controller, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';

const SelectField = ({
  name: controlName,
  label,
  options = [],
  InputLabelProps,
  FormControlProps,
  SelectProps,
  children,
}) => {
  const { control, errors, register } = useFormContext();
  const { t } = useTranslation();
  const randId = useMemo(
    () => uniqueId(`select_${kebabCase(label + controlName)}`),
    [controlName, label]
  );
  const errorMessage = get(errors, `${controlName}.message`, false);

  return (
    <FormControl error={!!errorMessage} fullWidth {...FormControlProps}>
      <InputLabel htmlFor={randId} id={randId} {...InputLabelProps}>
        {label}
      </InputLabel>
      <Controller
        control={control}
        name={controlName}
        render={({ value, name, onChange }) => (
          <Select
            labelId={name}
            name={name}
            id={randId}
            inputRef={register}
            value={value}
            fullWidth
            defaultValue
            onChange={onChange}
            {...SelectProps}
          >
            <MenuItem value="">
              <em>{t('noElementSelected')}</em>
            </MenuItem>
            {children
              ? children
              : options.map((option) => (
                  <MenuItem value={option.value}>{option.label}</MenuItem>
                ))}
          </Select>
        )}
      ></Controller>
      <FormHelperText error>{errorMessage}</FormHelperText>
    </FormControl>
  );
};

export default SelectField;
```
