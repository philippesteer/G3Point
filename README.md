# G3Point

Granulometry from 3D Point clouds
> Based on Steer, Guerit et al. (2022)

![ForGitHub](https://user-images.githubusercontent.com/17555304/159018713-7272a95e-6400-4490-83f5-868248cffbcb.gif)

## Introduction

**G3Point** is a *Matlab* program which aims at automatically measuring the size, shape, and orientation of a large number of individual grains as detected from any type of 3D point clouds describing the topography of surfaces covered by sediments. This algorithm relies on 3 main phases:
1. Grain **segmentation** using a waterhsed algorithm
2. Grain **merging and cleaning**
3. Grain **fitting by geometrical models** including ellipsoids and cuboids

If you have any questions or remarks, please contact the developper:

Philippe Steer philippe.steer[at]univ-rennes1.fr

## Requirements

G3Point is plateform independent and requires Matlab, the Image Processing, Lidar, Parallel and Statistics toolboxes. G3Point also relies on the "geom3d" toolbox (https://www.mathworks.com/matlabcentral/fileexchange/24484-geom3d), written dy David Legland, the "Fitting quadratic curves and surfaces" toolbox (https://www.mathworks.com/matlabcentral/fileexchange/45356-fitting-quadratic-curves-and-surfaces), written by Levente Hunyadi, and the "Minimal Bounding Box" toolbox (https://www.mathworks.com/matlabcentral/fileexchange/18264-minimal-bounding-box?s_tid=srchtitle), written by Johannes Korsawe.

## Getting started

We recommend to work directly under the main folder G3Point where are located the main "G3Point.m" main function, as well as other major functions and the working directories "PointCloud", "Figure", "Utils", "Grain" and "Excel". Do not rename, move or delete these folders.

### Point clouds

G3Point is particularly suited, efficient and fast (few seconds) to deal with patch-scale (1-100 m2) point clouds, obtained either with terrestrial LiDAR or by SFM, with a typical resolution of ~0.1-1 cm/point and a total number of points around 1e6. Point clouds are expected to be beforehand cleaned from vegetation or other unwanted features.

### Workflow

G3Point automatically asks the user to provide with a point cloud (extension in .ply). We recommend to put this point cloud in the subfolder "PointCloud" of the folder "G3Point". G3Point will then create new directories with the same name than the point cloud under "Figure", "Grain" and "Excel" where it will saves the results of G3Point. During a G3Point run, figures will be shown and saved (in "Figure"), the statistical results on the grain size, shape and orientation distribution saved in an Excel table (in "Excel"), and the point clouds of the segmented grains created (in "Grain").

### Algorithm parameters

G3Point uses a different set of parameters for each point cloud considered. Parameters are provided in the "param.csv" file that is located under the "PointCloud" directory. Each line of the "param.csv" table starts with the name of the associated point clouds and then defines the value of the parameters that are used. See below a screenshot of a example line of this table:

![image](https://user-images.githubusercontent.com/17555304/159018157-c9874503-9a90-47a1-aca1-7ff0b6337085.png)

We now briefly describe the meaning and role of each parameter

    Name                               % Name of the point cloud (including its extension) 
    % Yes (=1) or No (=0) parameters
    iplot                              % Plot results
    saveplot                           % Save plot
    denoise                            % Denoise the point cloud
    decimate                           % Decimate the point cloud
    minima                             % Remove local minima to ease segmentation
    rotdetrend                         % Rotate and detrend the point cloud
    clean                              % Cleaning segmentation
    gridbynumber                       % Perform a virtual grid-by-number grain size distribution
    savegranulo                        % Export granulometry to a xls file
    savegrain                          % Export grain point cloud in .ply
    % To decimate the point cloud (used if decimate==1)   
    res                                % New resolution of the point cloud
    % To remove local minima before segmentation (used if minima==1) !! TIME-CONSUMING !!
    nscale                             % Number of scale to use
    minscale                           % Minimum scale(radius)
    maxscale                           % Maximum potential scale (radius)
    % Segmentation parameters
    nnptCloud                          % K nearest neighbours to route water with Fastscape
    cf                                 % Prefactor that is used to determine if two grains should be merged
    maxangle1                          % Maximum angle above which the normals between two labels are considered different
    % Cleaning segmentation parameters  (used if clean==1)
    maxangle2                          % Maximum angle above which the normals between two labels are considered different
    minflatness                        % Minimum flatness that should have a grain
    minnpoint=max(nnptCloud,nmin);     % Minimal number of points that should contain a grain
    % Fitting method    
    fitmethod                          % (['direct'] / 'simple' / 'koopmans' / 'inertia' / 'direct_iteratve'/ but 'direct' works much better - and yet use a strong constraint)
    Aquality_thresh                    % Minimum surface cover threshold for the ellipsoid (in %)
    % To perform a grid-by-number sampling (if gridbynumber==1)    
    mindiam                            % Minimum grain diameter considered
    naxis                              % Which axis to use ? (1=a-axis, 2=b-axis, 3=c-axis)
    dx_gbn                             % Grid spacing used for the grid-by-number sampling of grain for gsd
          
   ## Potential bugs and solutions
    
   If you stumbe upon the following error:
   
        Error using ellipsoid_distance
        Too many output arguments.

        Error in fitellipsoidtograins (line 75)
        [Ellipsoidm(j).d,Ellipsoidm(j).r2] = ellipsoid_distance(P(:,1),P(:,2),P(:,3),Ellipsoidm(j).p);

        Error in G3Point (line 90)
        [Ellipsoidm]=fitellipsoidtograins(Pebble,param,nlabels);

   The solution is (thanks very much to Kieran for pointing it out): Do not install the quadratic curves package as the ellipsoid_distance function in the current package on matlab DOES NOT return r2. Use the package that comes in the zip file which DOES return r2.
