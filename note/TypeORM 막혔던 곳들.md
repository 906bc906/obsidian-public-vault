---
title: TypeORM 막혔던 곳들
date: 2022-10-13T21:05:17+09:00
last_modified_at: 2022-10-18T14:59:39+09:00
---
## composite PK는 어떻게 갖게 하는가?

```ts
  @PrimaryColumn()
  uniqueId?: number
  @PrimaryColumn()
  platform?: string
```

그냥 두 개 지정하면 composite PK임

## Foreign Key는 어떻게 지정하는가?

키워드 Relations. OneToOne ~ ManyToMany

```ts
    @OneToOne(() => Photo)
    @JoinColumn()
    photo: Photo
```

이 경우 단방향.

```ts
@Entity()
export class PhotoMetadata {
    /* ... other columns */

    @OneToOne(() => Photo, (photo) => photo.metadata)
    @JoinColumn()
    photo: Photo
}

@Entity()
export class Photo {
    /* ... other columns */

    @OneToOne(() => PhotoMetadata, (photoMetadata) => photoMetadata.photo)
    metadata: PhotoMetadata
}
```

이렇게 양방향으로 걸 수 있음. 단방향 관계 시 JoinColumn을 들고 있는 쪽만 조회할 수 있음.

