---
title: "Material UI: a way to handle files with react-hook-form"
timestamp: 2021-01-24T00:43:16.932Z
---

[Many thanks to](https://dev.to/vibhanshu909/how-to-use-react-dropzone-with-react-hook-form-1omc)

```jsx
import { Grid } from '@material-ui/core';
import useStyles from 'components/ConnectivityChecker/styles';
import { get, partial } from 'lodash';
import React, { useEffect, useMemo } from 'react';
import { useDropzone } from 'react-dropzone';
import { useFormContext } from 'react-hook-form';
import DropAttachment from './DropAttachment';
import DropAttachmentSkeleton from './DropAttachmentSkeleton';
import DropButton from './DropButton';
import DropWrapper from './DropWrapper';

const DropField = ({
  name,
  accept,
  isLoading,
  label,
  hintText,
  id,
  children,
}) => {
  const classes = useStyles();
  const { register, setValue, errors, watch, unregister } = useFormContext();
  const { getRootProps, getInputProps, isDragActive } = useDropzone({
    onDrop: (files) => setValue(name, files),
    accept,
    previewsContainer: '.dropzone-preview',
  });

  const files = watch(name);

  const media = useMemo(() => {
    if (!files?.length) return undefined;
    const [file] = files;

    if (file instanceof File)
      return {
        type: file.type,
        title: file.name,
        src: URL.createObjectURL(file),
      };
  }, [files]);

  useEffect(() => {
    register(name);
    return () => {
      unregister(name);
    };
  }, [register, unregister, name]);

  return (
    <DropWrapper
      error={get(errors, [name, 'message'], false)}
      hintText={hintText}
      label={label}
      accept={accept}
      id={id}
    >
      <DropAttachmentSkeleton isLoading={isLoading}>
        {media ? (
          <Grid item md={12} className={classes.root}>
            <DropAttachment
              media={media}
              isLoading={isLoading}
              onRemove={partial(setValue, name, '')}
            />
            {children}
          </Grid>
        ) : (
          <Grid item {...getRootProps()} md={12} className={classes.root}>
            <DropButton isLoading={isLoading} isDragActive={isDragActive} />
            <input id={id} name={name} {...getInputProps()} />
          </Grid>
        )}
      </DropAttachmentSkeleton>
    </DropWrapper>
  );
};

export default DropField;
```
