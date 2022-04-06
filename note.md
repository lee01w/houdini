# Joy of Vex

## 2

    float d = length(@P);
    @P.y = sin(d);

## 3
clamp

    float d = length(@P);
    @P.y = clamp(d,1,2);
fit

    float d = length(@P);
    @P.y = fit(d,1,2,1,3);

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

## 7

    vector pos = minpos(1, @P);

vector  minpos(geometry, point)  
Returns the position of the closest point in the given geometry to the point.  
주어진 지오메트리에서  포인트에 가장 가까운 포인트의 위치를 ​​반환합니다.

    int pt = nearpoint(1, @P);

int  nearpoint(geometry, pt)  
Returns the number of the closest point on the geometry. This will only search against points, not the surface locations of the geometry.  
기하 도형에서 가장 가까운 점의 번호를 반환합니다. 이것은 지오메트리의 표면 위치가 아닌 점에 대해서만 검색합니다.

## 8  

    @Cd = noise(@P * chv('frequency') + chv('offset') + @Time);
    @Cd = curlnoise(@P * chv('frequency') + chv('offset') + @Time);

## 9

normalize : 크기 1짜리 vector  
dot : 내적, 각도 크기 float  

    vector pos = point(1,'P',0);
    pos = normalize(pos);
    @Cd = dot(@N, pos);

cross : 외적, 수직이 되는 방향 vector

    @N = cross(@N, {0,1,0});

## 10

    vextor rel = relpointbbox(0, @P);
