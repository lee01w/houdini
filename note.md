# Joy of Vex

## 2

    float d = length(@P);
    @P.y = sin(d);

## 4

    float d = length(@P);
    @P.y = chramp('myramp', d);

## 5-1

    // float height = abs(@P.x);
    // float height = floor(@P.x);
    // float height = ceil(@P.x);
    // float height = rint(@P.x);
    float height = trunc(@P.x);
    @P.y = height;

## 5-2

    @Cd.r = @Time % 1;

## 5-3

    float d = length(@P);
    d /= ch('rad');
    d = trunc(d);
    d *= ch('height');
    @P.y = d;

## 6

Primitives wrangle에서의 @P는 읽기만 가능
