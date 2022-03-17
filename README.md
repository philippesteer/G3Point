# G3Point

Granulometry from 3D Point clouds
> Based on Steer, Guerit et al. (2022)

## Introduction

G3Point is a *Matlab* program which aims at automatically measuring the size, shape, and orientation of a large number of individual grains as detected from any type of 3D point clouds describing the topography of surfaces covered by sediments. This algorithm relies on 3 main phases:
1. Grain **segmentation** using a waterhsed algorithm
2. Grain **merging and cleaning**
3. Grain **fitting by geometrical models** including ellipsoids and cuboids

## Point clouds

G3Point is particularly suited, efficient and fast (few seconds) to deal with **patch-scale (1-100 $m^
2$) point clouds**, obtained either with terrestrial LiDAR or by SFM, with a typical resolution of ~0.1-1 cm/point and a total number of points around $10^6$. Point clouds are expected to be beforehand cleaned from vegetation or other unwanted features.

## Workflow

G3Point automatically asks the user to provide with a point cloud (extension in .ply). We recommend to put this point cloud in the subfolder "PointCloud" of the folder "G3Point". G3Point will then create new directories with the same name than the point cloud under "Figure", "Grain" and "Excel" where it will saves the results of G3Point. During a G3Point run, figures will be shown and saved (in "Figure"), the statistical results on the grain size, shape and orientation distribution saved in an Excel table (in "Excel"), and the point clouds of the segmented grains created (in "Grain").

## Algorithm parameters

    % Yes (=1) or No (=0) parameters
    param.iplot           = b;               % Plot results
    param.saveplot        = c;               % Save plot
    param.denoise         = d;               % Denoise the point cloud
    param.decimate        = e;               % Decimate the point cloud
    param.minima          = f;               % Remove local minima to ease segmentation
    param.rotdetrend      = g;               % Rotate and detrend the point cloud
    param.clean           = h;               % Cleaning segmentation
    param.gridbynumber    = i;               % Perform a virtual grid-by-number grain size distribution
    param.savegranulo     = j;               % Export granulometry to a xls file
    param.savegrain       = k;               % Export grain point cloud in .ply
    % To decimate the point cloud (used if para.decimate==1)   
    param.res             = l;               % New resolution of the point cloud
    % To remove local minima before segmentation (used if param.minima==1) !! TIME-CONSUMING !!
    param.nscale          = m;               % Number of scale to use
    param.minscale        = n;               % Minimum scale(radius)
    param.maxscale        = o;               % Maximum potential scale (radius)
    % Segmentation parameters
    param.nnptCloud       = p;               % K nearest neighbours to route water with Fastscape
    param.radfactor       = q;               % Prefactor that is used to determine if two grains should be merged
    param.maxangle1       = r;               % Maximum angle above which the normals between two labels are considered different
    % Cleaning segmentation parameters  (used if param.clean==1)
    param.maxangle2       = s;               % Maximum angle above which the normals between two labels are considered different
    param.minflatness     = t;               % Minimum flatness that should have a grain
    param.minnpoint=max(param.nnptCloud,y);  % Minimal number of points that should contain a grain
    % Fitting method    
    param.fitmethod       = char(u);         % (['direct'] / 'simple' / 'koopmans' / 'inertia' / 'direct_iteratve'/ but 'direct' works much better - and yet use a strong constraint)
    param.Aquality_thresh = v;               % Minimum surface cover threshold for the ellipsoid (in %)
    % To perform a grid-by-number sampling (if param.gridbynumber==1)    
    param.mindiam         = w;               % Minimum grain diameter considered
    param.naxis           = x;               % which axis to use ? (1=a-axis, 2=b-axis, 3=c-axis)
    param.dx_gbn          = z;               % grid spacing used for the grid-by-number sampling of grain for gsd
